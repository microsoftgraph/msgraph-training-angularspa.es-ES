<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="99f19-101">En este ejercicio, extenderá la aplicación desde el ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="99f19-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="99f19-102">Esto es necesario para obtener el token de acceso OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="99f19-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="99f19-103">En este paso, integrará la Biblioteca de [autenticación](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) de Microsoft para Angular en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="99f19-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

1. <span data-ttu-id="99f19-104">Cree un nuevo archivo en el directorio **./src** denominado **oauth.ts** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="99f19-104">Create a new file in the **./src** directory named **oauth.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.example.ts":::

    <span data-ttu-id="99f19-105">Reemplace `YOUR_APP_ID_HERE` por el identificador de aplicación del Portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="99f19-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="99f19-106">Si usas el control de código fuente como git, ahora sería un buen momento para excluir el archivo **oauth.ts** del control de código fuente para evitar la pérdida involuntaria del identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="99f19-106">If you're using source control such as git, now would be a good time to exclude the **oauth.ts** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="99f19-107">Abre **./src/app/app.module.ts** y agrega las siguientes `import` instrucciones a la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="99f19-107">Open **./src/app/app.module.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { IPublicClientApplication,
             PublicClientApplication,
             BrowserCacheLocation } from '@azure/msal-browser';
    import { MsalModule,
             MsalService,
             MSAL_INSTANCE } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. <span data-ttu-id="99f19-108">Agregue la siguiente función debajo `import` de las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="99f19-108">Add the following function below the `import` statements.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="MSALFactorySnippet":::

1. <span data-ttu-id="99f19-109">Agregue la `MsalModule` matriz `imports` dentro de la `@NgModule` declaración.</span><span class="sxs-lookup"><span data-stu-id="99f19-109">Add the `MsalModule` to the `imports` array inside the `@NgModule` declaration.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ImportsSnippet" highlight="6":::

1. <span data-ttu-id="99f19-110">Agregue el `MSALInstanceFactory` y `MsalService` a la `providers` matriz dentro de la `@NgModule` declaración.</span><span class="sxs-lookup"><span data-stu-id="99f19-110">Add the `MSALInstanceFactory` and `MsalService` to the `providers` array inside the `@NgModule` declaration.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ProvidersSnippet" highlight="2-6":::

## <a name="implement-sign-in"></a><span data-ttu-id="99f19-111">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="99f19-111">Implement sign-in</span></span>

<span data-ttu-id="99f19-112">En esta sección, creará un servicio de autenticación e implementará el inicio de sesión y la salida.</span><span class="sxs-lookup"><span data-stu-id="99f19-112">In this section you'll create an authentication service and implement sign-in and sign-out.</span></span>

1. <span data-ttu-id="99f19-113">Ejecute el siguiente comando en la CLI.</span><span class="sxs-lookup"><span data-stu-id="99f19-113">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service auth
    ```

    <span data-ttu-id="99f19-114">Al crear un servicio para esto, puede insertarlo fácilmente en cualquier componente que necesite acceso a los métodos de autenticación.</span><span class="sxs-lookup"><span data-stu-id="99f19-114">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span>

1. <span data-ttu-id="99f19-115">Una vez que finalice el comando, abra **./src/app/auth.service.ts** y reemplace su contenido por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="99f19-115">Once the command finishes, open **./src/app/auth.service.ts** and replace its contents with the following code.</span></span>

    ```typescript
    import { Injectable } from '@angular/core';
    import { AccountInfo } from '@azure/msal-browser';
    import { MsalService } from '@azure/msal-angular';

    import { AlertsService } from './alerts.service';
    import { OAuthSettings } from '../oauth';
    import { User } from './user';

    @Injectable({
      providedIn: 'root'
    })

    export class AuthService {
      public authenticated: boolean;
      public user?: User;

      constructor(
        private msalService: MsalService,
        private alertsService: AlertsService) {

        this.authenticated = false;
        this.user = undefined;
      }

      // Prompt the user to sign in and
      // grant consent to the requested permission scopes
      async signIn(): Promise<void> {
        const result = await this.msalService
          .loginPopup(OAuthSettings)
          .toPromise()
          .catch((reason) => {
            this.alertsService.addError('Login failed',
              JSON.stringify(reason, null, 2));
          });

        if (result) {
          this.msalService.instance.setActiveAccount(result.account);
          this.authenticated = true;
          // Temporary placeholder
          this.user = new User();
          this.user.displayName = 'Adele Vance';
          this.user.email = 'AdeleV@contoso.com';
          this.user.avatar = '/assets/no-profile-photo.png';
        }
      }

      // Sign out
      async signOut(): Promise<void> {
        await this.msalService.logout().toPromise();
        this.user = undefined;
        this.authenticated = false;
      }

      // Silently request an access token
      async getAccessToken(): Promise<string> {
        const result = await this.msalService
          .acquireTokenSilent({
            scopes: OAuthSettings.scopes
          })
          .toPromise()
          .catch((reason) => {
            this.alertsService.addError('Get token failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          // Temporary to display token in an error box
          this.alertsService.addSuccess('Token acquired', result.accessToken);
          return result.accessToken;
        }

        // Couldn't get a token
        this.authenticated = false;
        return '';
      }
    }
    ```

1. <span data-ttu-id="99f19-116">Abra **./src/app/nav-bar/nav-bar.component.ts** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="99f19-116">Open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,34-36,38-40":::

1. <span data-ttu-id="99f19-117">Abra **./src/app/home/home.component.ts** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="99f19-117">Open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,13-20,22,26-33":::

<span data-ttu-id="99f19-118">Guarde los cambios y actualice el explorador.</span><span class="sxs-lookup"><span data-stu-id="99f19-118">Save your changes and refresh the browser.</span></span> <span data-ttu-id="99f19-119">Haga clic **en el botón Hacer clic aquí para iniciar sesión** y se le redirigirá a `https://login.microsoftonline.com` .</span><span class="sxs-lookup"><span data-stu-id="99f19-119">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="99f19-120">Inicie sesión con su cuenta de Microsoft y consiente los permisos solicitados.</span><span class="sxs-lookup"><span data-stu-id="99f19-120">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="99f19-121">La página de la aplicación debe actualizarse y mostrar el token.</span><span class="sxs-lookup"><span data-stu-id="99f19-121">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="99f19-122">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="99f19-122">Get user details</span></span>

<span data-ttu-id="99f19-123">En este momento, el servicio de autenticación establece valores constantes para el nombre para mostrar y la dirección de correo electrónico del usuario.</span><span class="sxs-lookup"><span data-stu-id="99f19-123">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="99f19-124">Ahora que tiene un token de acceso, puede obtener detalles de usuario de Microsoft Graph para que esos valores correspondan al usuario actual.</span><span class="sxs-lookup"><span data-stu-id="99f19-124">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span>

1. <span data-ttu-id="99f19-125">Abra **./src/app/auth.service.ts** y agregue las siguientes `import` instrucciones a la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="99f19-125">Open **./src/app/auth.service.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    ```

1. <span data-ttu-id="99f19-126">Agregue una nueva función llamada `AuthService` a la clase `getUser`.</span><span class="sxs-lookup"><span data-stu-id="99f19-126">Add a new function to the `AuthService` class called `getUser`.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="GetUserSnippet":::

1. <span data-ttu-id="99f19-127">Busque y quite el siguiente código en el `getAccessToken` método que agrega una alerta para mostrar el token de acceso.</span><span class="sxs-lookup"><span data-stu-id="99f19-127">Locate and remove the following code in the `getAccessToken` method that adds an alert to display the access token.</span></span>

    ```typescript
    // Temporary to display token in an error box
    this.alertsService.addSuccess('Token acquired', result);
    ```

1. <span data-ttu-id="99f19-128">Busque y quite el siguiente código del `signIn` método.</span><span class="sxs-lookup"><span data-stu-id="99f19-128">Locate and remove the following code from the `signIn` method.</span></span>

    ```typescript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    this.user.avatar = '/assets/no-profile-photo.png';
    ```

1. <span data-ttu-id="99f19-129">En su lugar, agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="99f19-129">In its place, add the following code.</span></span>

    ```typescript
    this.user = await this.getUser();
    ```

    <span data-ttu-id="99f19-130">Este nuevo código usa el SDK de Microsoft Graph para obtener los detalles del usuario y, a continuación, crea un objeto con valores devueltos `User` por la llamada a la API.</span><span class="sxs-lookup"><span data-stu-id="99f19-130">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

1. <span data-ttu-id="99f19-131">Cambie la clase para comprobar si el usuario ya ha iniciado sesión y `constructor` cargue sus detalles si es `AuthService` así.</span><span class="sxs-lookup"><span data-stu-id="99f19-131">Change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="99f19-132">Reemplace el existente `constructor` por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="99f19-132">Replace the existing `constructor` with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="ConstructorSnippet" highlight="5-7":::

1. <span data-ttu-id="99f19-133">Quite el código temporal de la `HomeComponent` clase.</span><span class="sxs-lookup"><span data-stu-id="99f19-133">Remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="99f19-134">Abra **./src/app/home/home.component.ts** y reemplace la función `signIn` existente por la siguiente.</span><span class="sxs-lookup"><span data-stu-id="99f19-134">Open **./src/app/home/home.component.ts** and replace the existing `signIn` function with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="SignInSnippet":::

<span data-ttu-id="99f19-135">Ahora, si guarda los cambios e inicia la aplicación, después del inicio de sesión debe volver a la página principal, pero la interfaz de usuario debe cambiar para indicar que ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="99f19-135">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Una captura de pantalla de la página de inicio después de iniciar sesión](./images/add-aad-auth-01.png)

<span data-ttu-id="99f19-137">Haga clic en el avatar del usuario en la esquina superior derecha para obtener acceso al vínculo **Cerrar sesión.**</span><span class="sxs-lookup"><span data-stu-id="99f19-137">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="99f19-138">Al **hacer clic en** Cerrar sesión, se restablece la sesión y se devuelve a la página principal.</span><span class="sxs-lookup"><span data-stu-id="99f19-138">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Una captura de pantalla del menú desplegable con el vínculo Cerrar sesión](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="99f19-140">Almacenar y actualizar tokens</span><span class="sxs-lookup"><span data-stu-id="99f19-140">Storing and refreshing tokens</span></span>

<span data-ttu-id="99f19-141">En este momento, la aplicación tiene un token de acceso, que se envía en el `Authorization` encabezado de las llamadas API.</span><span class="sxs-lookup"><span data-stu-id="99f19-141">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="99f19-142">Este es el token que permite a la aplicación tener acceso a Microsoft Graph en nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="99f19-142">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="99f19-143">Sin embargo, este token es de corta duración.</span><span class="sxs-lookup"><span data-stu-id="99f19-143">However, this token is short-lived.</span></span> <span data-ttu-id="99f19-144">El token expira una hora después de su emisión.</span><span class="sxs-lookup"><span data-stu-id="99f19-144">The token expires an hour after it is issued.</span></span> <span data-ttu-id="99f19-145">Dado que la aplicación usa la biblioteca MSAL, no es necesario implementar ninguna lógica de actualización o almacenamiento de tokens.</span><span class="sxs-lookup"><span data-stu-id="99f19-145">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="99f19-146">El `MsalService` token se almacena en caché en el almacenamiento del explorador.</span><span class="sxs-lookup"><span data-stu-id="99f19-146">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="99f19-147">El método primero comprueba el token almacenado en caché y, si no ha `acquireTokenSilent` expirado, lo devuelve.</span><span class="sxs-lookup"><span data-stu-id="99f19-147">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="99f19-148">Si ha expirado, realiza una solicitud silenciosa para obtener una nueva.</span><span class="sxs-lookup"><span data-stu-id="99f19-148">If it is expired, it makes a silent request to obtain a new one.</span></span>
