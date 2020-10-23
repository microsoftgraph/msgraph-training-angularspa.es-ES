<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fc63d-101">En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fc63d-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="fc63d-102">Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="fc63d-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="fc63d-103">En este paso, integrará la [biblioteca de autenticación de Microsoft para angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fc63d-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

1. <span data-ttu-id="fc63d-104">Cree un nuevo archivo en el directorio **./src** denominado **OAuth. ts** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="fc63d-104">Create a new file in the **./src** directory named **oauth.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.example.ts":::

    <span data-ttu-id="fc63d-105">Reemplace `YOUR_APP_ID_HERE` por el identificador de la aplicación del portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="fc63d-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="fc63d-106">Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el archivo **OAuth. ts** del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fc63d-106">If you're using source control such as git, now would be a good time to exclude the **oauth.ts** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="fc63d-107">Abra **./src/app/app.Module.ts** y agregue las siguientes `import` instrucciones en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="fc63d-107">Open **./src/app/app.module.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { MsalModule } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. <span data-ttu-id="fc63d-108">Agregue el `MsalModule` a la `imports` matriz dentro de la `@NgModule` declaración e INICIALÍCELA con el identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fc63d-108">Add the `MsalModule` to the `imports` array inside the `@NgModule` declaration, and initialize it with the app ID.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="imports" highlight="6-11":::

## <a name="implement-sign-in"></a><span data-ttu-id="fc63d-109">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="fc63d-109">Implement sign-in</span></span>

<span data-ttu-id="fc63d-110">En esta sección, creará un servicio de autenticación e implementará el inicio y el cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="fc63d-110">In this section you'll create an authentication service and implement sign-in and sign-out.</span></span>

1. <span data-ttu-id="fc63d-111">Ejecute el siguiente comando en su CLI.</span><span class="sxs-lookup"><span data-stu-id="fc63d-111">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service auth
    ```

    <span data-ttu-id="fc63d-112">Al crear un servicio para esto, puede inyectarlo fácilmente en cualquier componente que necesite acceso a los métodos de autenticación.</span><span class="sxs-lookup"><span data-stu-id="fc63d-112">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span>

1. <span data-ttu-id="fc63d-113">Una vez que finalice el comando, Abra **./src/App/auth.Service.ts** y reemplace el contenido por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="fc63d-113">Once the command finishes, open **./src/app/auth.service.ts** and replace its contents with the following code.</span></span>

    ```typescript
    import { Injectable } from '@angular/core';
    import { MsalService } from '@azure/msal-angular';

    import { AlertsService } from './alerts.service';
    import { OAuthSettings } from '../oauth';
    import { User } from './user';

    @Injectable({
      providedIn: 'root'
    })

    export class AuthService {
      public authenticated: boolean;
      public user: User;

      constructor(
        private msalService: MsalService,
        private alertsService: AlertsService) {

        this.authenticated = false;
        this.user = null;
      }

      // Prompt the user to sign in and
      // grant consent to the requested permission scopes
      async signIn(): Promise<void> {
        let result = await this.msalService.loginPopup(OAuthSettings)
          .catch((reason) => {
            this.alertsService.addError('Login failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          this.authenticated = true;
          // Temporary placeholder
          this.user = new User();
          this.user.displayName = 'Adele Vance';
          this.user.email = 'AdeleV@contoso.com';
          this.user.avatar = '/assets/no-profile-photo.png';
        }
      }

      // Sign out
      signOut(): void {
        this.msalService.logout();
        this.user = null;
        this.authenticated = false;
      }

      // Silently request an access token
      async getAccessToken(): Promise<string> {
        let result = await this.msalService.acquireTokenSilent(OAuthSettings)
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
        return null;
      }
    }
    ```

1. <span data-ttu-id="fc63d-114">Abra **./src/App/NAV-Bar/Nav-bar.Component.ts** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="fc63d-114">Open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,26-28,36-38,40-42":::

1. <span data-ttu-id="fc63d-115">Abra **./src/App/Home/Home.Component.ts** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="fc63d-115">Open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,12-19,21,23,25-32":::

<span data-ttu-id="fc63d-116">Guarde los cambios y actualice el explorador.</span><span class="sxs-lookup"><span data-stu-id="fc63d-116">Save your changes and refresh the browser.</span></span> <span data-ttu-id="fc63d-117">Haga clic en el botón **haga clic aquí para iniciar sesión** y debe redirigirse a `https://login.microsoftonline.com` .</span><span class="sxs-lookup"><span data-stu-id="fc63d-117">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="fc63d-118">Inicie sesión con su cuenta de Microsoft y dé su consentimiento a los permisos solicitados.</span><span class="sxs-lookup"><span data-stu-id="fc63d-118">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="fc63d-119">La página de la aplicación debe actualizarse y mostrar el token.</span><span class="sxs-lookup"><span data-stu-id="fc63d-119">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="fc63d-120">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="fc63d-120">Get user details</span></span>

<span data-ttu-id="fc63d-121">Ahora, el servicio de autenticación establece valores constantes para el nombre para mostrar y la dirección de correo electrónico del usuario.</span><span class="sxs-lookup"><span data-stu-id="fc63d-121">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="fc63d-122">Ahora que tiene un token de acceso, puede obtener detalles del usuario de Microsoft Graph para que esos valores se correspondan con el usuario actual.</span><span class="sxs-lookup"><span data-stu-id="fc63d-122">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span>

1. <span data-ttu-id="fc63d-123">Abra **./src/App/auth.Service.ts** y agregue las siguientes `import` instrucciones en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="fc63d-123">Open **./src/app/auth.service.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    ```

1. <span data-ttu-id="fc63d-124">Agregue una nueva función llamada `AuthService` a la clase `getUser`.</span><span class="sxs-lookup"><span data-stu-id="fc63d-124">Add a new function to the `AuthService` class called `getUser`.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="getUserSnippet":::

1. <span data-ttu-id="fc63d-125">Busque y quite el código siguiente en el `getAccessToken` método que agrega una alerta para mostrar el token de acceso.</span><span class="sxs-lookup"><span data-stu-id="fc63d-125">Locate and remove the following code in the `getAccessToken` method that adds an alert to display the access token.</span></span>

    ```typescript
    // Temporary to display token in an error box
    this.alertsService.addSuccess('Token acquired', result);
    ```

1. <span data-ttu-id="fc63d-126">Busque y quite el código siguiente del `signIn` método.</span><span class="sxs-lookup"><span data-stu-id="fc63d-126">Locate and remove the following code from the `signIn` method.</span></span>

    ```typescript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    this.user.avatar = '/assets/no-profile-photo.png';
    ```

1. <span data-ttu-id="fc63d-127">En su ubicación, agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="fc63d-127">In its place, add the following code.</span></span>

    ```typescript
    this.user = await this.getUser();
    ```

    <span data-ttu-id="fc63d-128">Este nuevo código usa el SDK de Microsoft Graph para obtener los detalles del usuario y, a continuación, crea un `User` objeto con los valores devueltos por la llamada a la API.</span><span class="sxs-lookup"><span data-stu-id="fc63d-128">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

1. <span data-ttu-id="fc63d-129">Cambie la `constructor` para la `AuthService` clase para comprobar si el usuario ya ha iniciado sesión y cargar sus detalles en caso afirmativo.</span><span class="sxs-lookup"><span data-stu-id="fc63d-129">Change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="fc63d-130">Reemplace el existente `constructor` por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="fc63d-130">Replace the existing `constructor` with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="constructorSnippet" highlight="5-6":::

1. <span data-ttu-id="fc63d-131">Quite el código temporal de la `HomeComponent` clase.</span><span class="sxs-lookup"><span data-stu-id="fc63d-131">Remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="fc63d-132">Abra **./src/App/Home/Home.Component.ts** y reemplace la `signIn` función existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="fc63d-132">Open **./src/app/home/home.component.ts** and replace the existing `signIn` function with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="signInSnippet":::

<span data-ttu-id="fc63d-133">Ahora, si guarda los cambios e inicia la aplicación, después de iniciar sesión debe terminar de nuevo en la Página principal, pero la interfaz de usuario debe cambiar para indicar que ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="fc63d-133">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Una captura de pantalla de la Página principal después de iniciar sesión](./images/add-aad-auth-01.png)

<span data-ttu-id="fc63d-135">Haga clic en el avatar de usuario en la esquina superior derecha para acceder al vínculo **Cerrar sesión** .</span><span class="sxs-lookup"><span data-stu-id="fc63d-135">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="fc63d-136">Al hacer clic en **cerrar** sesión se restablece la sesión y se vuelve a la Página principal.</span><span class="sxs-lookup"><span data-stu-id="fc63d-136">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Captura de pantalla del menú desplegable con el vínculo cerrar sesión](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="fc63d-138">Almacenamiento y actualización de tokens</span><span class="sxs-lookup"><span data-stu-id="fc63d-138">Storing and refreshing tokens</span></span>

<span data-ttu-id="fc63d-139">En este punto, la aplicación tiene un token de acceso, que se envía en el `Authorization` encabezado de las llamadas a la API.</span><span class="sxs-lookup"><span data-stu-id="fc63d-139">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="fc63d-140">Este es el token que permite que la aplicación tenga acceso a Microsoft Graph en nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="fc63d-140">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="fc63d-141">Sin embargo, este token es de corta duración.</span><span class="sxs-lookup"><span data-stu-id="fc63d-141">However, this token is short-lived.</span></span> <span data-ttu-id="fc63d-142">El token expira una hora después de su emisión.</span><span class="sxs-lookup"><span data-stu-id="fc63d-142">The token expires an hour after it is issued.</span></span> <span data-ttu-id="fc63d-143">Debido a que la aplicación usa la biblioteca de MSAL, no tiene que implementar ninguna lógica de almacenamiento o actualización de tokens.</span><span class="sxs-lookup"><span data-stu-id="fc63d-143">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="fc63d-144">El `MsalService` almacena en caché el token en el almacenamiento del explorador.</span><span class="sxs-lookup"><span data-stu-id="fc63d-144">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="fc63d-145">El `acquireTokenSilent` método comprueba primero el token almacenado en caché y, si no lo ha expirado, lo devuelve.</span><span class="sxs-lookup"><span data-stu-id="fc63d-145">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="fc63d-146">Si ha expirado, realiza una solicitud silenciosa para obtener uno nuevo.</span><span class="sxs-lookup"><span data-stu-id="fc63d-146">If it is expired, it makes a silent request to obtain a new one.</span></span>
