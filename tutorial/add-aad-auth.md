<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fcab1-101">En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fcab1-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="fcab1-102">Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="fcab1-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="fcab1-103">En este paso, integrará la [biblioteca de autenticación de Microsoft para angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fcab1-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

<span data-ttu-id="fcab1-104">Cree un nuevo archivo en el `./src` directorio denominado `oauth.ts` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="fcab1-104">Create a new file in the `./src` directory named `oauth.ts` and add the following code.</span></span>

```TypeScript
export const OAuthSettings = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

<span data-ttu-id="fcab1-105">Reemplace `YOUR_APP_ID_HERE` por el identificador de la aplicación del portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="fcab1-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fcab1-106">Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el `oauth.ts` archivo del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fcab1-106">If you're using source control such as git, now would be a good time to exclude the `oauth.ts` file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="fcab1-107">Abra `./src/app/app.module.ts` y agregue las siguientes `import` instrucciones en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="fcab1-107">Open `./src/app/app.module.ts` and add the following `import` statements to the top of the file.</span></span>

```TypeScript
import { MsalModule } from '@azure/msal-angular';
import { OAuthSettings } from '../oauth';
```

<span data-ttu-id="fcab1-108">A continuación, `MsalModule` agregue a `imports` la matriz dentro `@NgModule` de la declaración e inicialícela con el identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fcab1-108">Then add the `MsalModule` to the `imports` array inside the `@NgModule` declaration, and initialize it with the app ID.</span></span>

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule,
  MsalModule.forRoot({
    clientID: OAuthSettings.appId
  })
],
```

## <a name="implement-sign-in"></a><span data-ttu-id="fcab1-109">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="fcab1-109">Implement sign-in</span></span>

<span data-ttu-id="fcab1-110">Empiece por definir una clase `User` sencilla para contener la información sobre el usuario que muestra la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fcab1-110">Start by defining a simple `User` class to hold the information about the user that the app displays.</span></span> <span data-ttu-id="fcab1-111">Cree un nuevo archivo en la `./src/app` carpeta denominada `user.ts` y agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="fcab1-111">Create a new file in the `./src/app` folder named `user.ts` and add the following code.</span></span>

```TypeScript
export class User {
  displayName: string;
  email: string;
  avatar: string;
}
```

<span data-ttu-id="fcab1-112">Ahora cree un servicio de autenticación.</span><span class="sxs-lookup"><span data-stu-id="fcab1-112">Now create an authentication service.</span></span> <span data-ttu-id="fcab1-113">Al crear un servicio para esto, puede inyectarlo fácilmente en cualquier componente que necesite acceso a los métodos de autenticación.</span><span class="sxs-lookup"><span data-stu-id="fcab1-113">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span> <span data-ttu-id="fcab1-114">Ejecute el siguiente comando en su CLI.</span><span class="sxs-lookup"><span data-stu-id="fcab1-114">Run the following command in your CLI.</span></span>

```Shell
ng generate service auth
```

<span data-ttu-id="fcab1-115">Una vez que finalice el comando, `./src/app/auth.service.ts` Abra el archivo y reemplace el contenido por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="fcab1-115">Once the command finishes, open the `./src/app/auth.service.ts` file and replace its contents with the following code.</span></span>

```TypeScript
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
    let result = await this.msalService.loginPopup(OAuthSettings.scopes)
      .catch((reason) => {
        this.alertsService.add('Login failed', JSON.stringify(reason, null, 2));
      });

    if (result) {
      this.authenticated = true;
      // Temporary placeholder
      this.user = new User();
      this.user.displayName = "Adele Vance";
      this.user.email = "AdeleV@contoso.com";
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
    let result = await this.msalService.acquireTokenSilent(OAuthSettings.scopes)
      .catch((reason) => {
        this.alertsService.add('Get token failed', JSON.stringify(reason, null, 2));
      });

    // Temporary to display token in an error box
    if (result) this.alertsService.add('Token acquired', result);
    return result;
  }
}
```

<span data-ttu-id="fcab1-116">Ahora que tiene el servicio de autenticación, se puede inyectar en los componentes que inician sesión.</span><span class="sxs-lookup"><span data-stu-id="fcab1-116">Now that you have the authentication service, it can be injected into the components that do sign-in.</span></span> <span data-ttu-id="fcab1-117">Comience con el `NavBarComponent`.</span><span class="sxs-lookup"><span data-stu-id="fcab1-117">Start with the `NavBarComponent`.</span></span> <span data-ttu-id="fcab1-118">Abra el `./src/app/nav-bar/nav-bar.component.ts` archivo y realice los cambios siguientes.</span><span class="sxs-lookup"><span data-stu-id="fcab1-118">Open the `./src/app/nav-bar/nav-bar.component.ts` file and make the following changes.</span></span>

- <span data-ttu-id="fcab1-119">Agregar `import { AuthService } from '../auth.service';` a la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="fcab1-119">Add `import { AuthService } from '../auth.service';` to the top of the file.</span></span>
- <span data-ttu-id="fcab1-120">Quite las `authenticated` propiedades `user` y de la clase y quite el código que las establece en `ngOnInit`.</span><span class="sxs-lookup"><span data-stu-id="fcab1-120">Remove the `authenticated` and `user` properties from the class, and remove the code that sets them in `ngOnInit`.</span></span>
- <span data-ttu-id="fcab1-121">Inserte el `AuthService` mediante la adición del siguiente parámetro a `constructor`: `private authService: AuthService`.</span><span class="sxs-lookup"><span data-stu-id="fcab1-121">Inject the `AuthService` by adding the following parameter to the `constructor`: `private authService: AuthService`.</span></span>
- <span data-ttu-id="fcab1-122">Reemplace el método `signIn` existente por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="fcab1-122">Replace the existing `signIn` method with the following:</span></span>

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();
    }
    ```

- <span data-ttu-id="fcab1-123">Reemplace el método `signOut` existente por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="fcab1-123">Replace the existing `signOut` method with the following:</span></span>

    ```TypeScript
    signOut(): void {
      this.authService.signOut();
    }
    ```

<span data-ttu-id="fcab1-124">Cuando termine, el código debe tener un aspecto similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="fcab1-124">When you're done, the code should look like the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';

import { AuthService } from '../auth.service';

@Component({
  selector: 'app-nav-bar',
  templateUrl: './nav-bar.component.html',
  styleUrls: ['./nav-bar.component.css']
})
export class NavBarComponent implements OnInit {

  // Should the collapsed nav show?
  showNav: boolean;

  constructor(private authService: AuthService) { }

  ngOnInit() {
    this.showNav = false;
  }

  // Used by the Bootstrap navbar-toggler button to hide/show
  // the nav in a collapsed state
  toggleNavBar(): void {
    this.showNav = !this.showNav;
  }

  async signIn(): Promise<void> {
    await this.authService.signIn();
  }

  signOut(): void {
    this.authService.signOut();
  }
}
```

<span data-ttu-id="fcab1-125">Como ha quitado `authenticated` las `user` propiedades y de la clase, también tiene que actualizar el `./src/app/nav-bar/nav-bar.component.html` archivo.</span><span class="sxs-lookup"><span data-stu-id="fcab1-125">Since you removed the `authenticated` and `user` properties on the class, you also need to update the `./src/app/nav-bar/nav-bar.component.html` file.</span></span> <span data-ttu-id="fcab1-126">Abra el archivo y realice los cambios siguientes.</span><span class="sxs-lookup"><span data-stu-id="fcab1-126">Open that file and make the following changes.</span></span>

- <span data-ttu-id="fcab1-127">Reemplace todas las instancias de `authenticated` por `authService.authenticated`.</span><span class="sxs-lookup"><span data-stu-id="fcab1-127">Replace all instances of `authenticated` with `authService.authenticated`.</span></span>
- <span data-ttu-id="fcab1-128">Reemplace todas las instancias `user` de `authService.user`por.</span><span class="sxs-lookup"><span data-stu-id="fcab1-128">Replace all instance of `user` with `authService.user`.</span></span>

<span data-ttu-id="fcab1-129">A continuación, `HomeComponent` actualice la clase.</span><span class="sxs-lookup"><span data-stu-id="fcab1-129">Next update the `HomeComponent` class.</span></span> <span data-ttu-id="fcab1-130">Realice todos los cambios que realizó en `./src/app/home/home.component.ts` la `NavBarComponent` clase con las siguientes excepciones.</span><span class="sxs-lookup"><span data-stu-id="fcab1-130">Make all of the same changes in `./src/app/home/home.component.ts` that you made to the `NavBarComponent` class with the following exceptions.</span></span>

- <span data-ttu-id="fcab1-131">No hay ningún `signOut` método en la `HomeComponent` clase.</span><span class="sxs-lookup"><span data-stu-id="fcab1-131">There is no `signOut` method in the `HomeComponent` class.</span></span>
- <span data-ttu-id="fcab1-132">Reemplace el `signIn` método por una versión ligeramente distinta.</span><span class="sxs-lookup"><span data-stu-id="fcab1-132">Replace the `signIn` method with a slightly different version.</span></span> <span data-ttu-id="fcab1-133">Este código llama `getAccessToken` a obtener un token de acceso, que, por ahora, generará el token como un error.</span><span class="sxs-lookup"><span data-stu-id="fcab1-133">This code calls `getAccessToken` to get an access token, which, for now, will output the token as an error.</span></span>

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();

      // Temporary to display the token
      if (this.authService.authenticated) {
        let token = await this.authService.getAccessToken();
      }
    }
    ```

<span data-ttu-id="fcab1-134">Cuando termine, el archivo debe tener un aspecto similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="fcab1-134">When your done, the file should look like the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';
import { AuthService } from '../auth.service';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {

  constructor(private authService: AuthService) { }

  ngOnInit() {
  }

  async signIn(): Promise<void> {
    await this.authService.signIn();

    // Temporary to display the token
    if (this.authService.authenticated) {
      let token = await this.authService.getAccessToken();
    }
  }
}
```

<span data-ttu-id="fcab1-135">Por último, realice los mismos reemplazos en `./src/app/home/home.component.html` que usted realizó para la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="fcab1-135">Finally, make the same replacements in `./src/app/home/home.component.html` that you made for the nav bar.</span></span>

<span data-ttu-id="fcab1-136">Guarde los cambios y actualice el explorador.</span><span class="sxs-lookup"><span data-stu-id="fcab1-136">Save your changes and refresh the browser.</span></span> <span data-ttu-id="fcab1-137">Haga clic en el botón **haga clic aquí para iniciar sesión** y debe redirigirse a `https://login.microsoftonline.com`.</span><span class="sxs-lookup"><span data-stu-id="fcab1-137">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="fcab1-138">Inicie sesión con su cuenta de Microsoft y dé su consentimiento a los permisos solicitados.</span><span class="sxs-lookup"><span data-stu-id="fcab1-138">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="fcab1-139">La página de la aplicación debe actualizarse y mostrar el token.</span><span class="sxs-lookup"><span data-stu-id="fcab1-139">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="fcab1-140">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="fcab1-140">Get user details</span></span>

<span data-ttu-id="fcab1-141">Ahora, el servicio de autenticación establece valores constantes para el nombre para mostrar y la dirección de correo electrónico del usuario.</span><span class="sxs-lookup"><span data-stu-id="fcab1-141">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="fcab1-142">Ahora que tiene un token de acceso, puede obtener detalles del usuario de Microsoft Graph para que esos valores se correspondan con el usuario actual.</span><span class="sxs-lookup"><span data-stu-id="fcab1-142">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span> <span data-ttu-id="fcab1-143">Abra `./src/app/auth.service.ts` y agregue la siguiente `import` instrucción a la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="fcab1-143">Open `./src/app/auth.service.ts` and add the following `import` statement to the top of the file.</span></span>

```TypeScript
import { Client } from '@microsoft/microsoft-graph-client';
```

<span data-ttu-id="fcab1-144">Agregue una nueva función llamada `AuthService` a la clase `getUser`.</span><span class="sxs-lookup"><span data-stu-id="fcab1-144">Add a new function to the `AuthService` class called `getUser`.</span></span>

```TypeScript
private async getUser(): Promise<User> {
  if (!this.authenticated) return null;

  let graphClient = Client.init({
    // Initialize the Graph client with an auth
    // provider that requests the token from the
    // auth service
    authProvider: async(done) => {
      let token = await this.getAccessToken()
        .catch((reason) => {
          done(reason, null);
        });

      if (token)
      {
        done(null, token);
      } else {
        done("Could not get an access token", null);
      }
    }
  });

  // Get the user from Graph (GET /me)
  let graphUser = await graphClient.api('/me').get();

  let user = new User();
  user.displayName = graphUser.displayName;
  // Prefer the mail property, but fall back to userPrincipalName
  user.email = graphUser.mail || graphUser.userPrincipalName;

  return user;
}
```

<span data-ttu-id="fcab1-145">Busque y quite el código siguiente del `signIn` método.</span><span class="sxs-lookup"><span data-stu-id="fcab1-145">Locate and remove the following code from the `signIn` method.</span></span>

```TypeScript
// Temporary placeholder
this.user = new User();
this.user.displayName = "Adele Vance";
this.user.email = "AdeleV@contoso.com";
```

<span data-ttu-id="fcab1-146">En su ubicación, agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="fcab1-146">In its place, add the following code.</span></span>

```TypeScript
this.user = await this.getUser();
```

<span data-ttu-id="fcab1-147">Este nuevo código usa el SDK de Microsoft Graph para obtener los detalles del usuario y, a `User` continuación, crea un objeto con los valores devueltos por la llamada a la API.</span><span class="sxs-lookup"><span data-stu-id="fcab1-147">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

<span data-ttu-id="fcab1-148">Ahora, cambie `constructor` la para `AuthService` la clase para comprobar si el usuario ya ha iniciado sesión y cargar sus detalles en caso afirmativo.</span><span class="sxs-lookup"><span data-stu-id="fcab1-148">Now change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="fcab1-149">Reemplace el existente `constructor` por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="fcab1-149">Replace the existing `constructor` with the following.</span></span>

```TypeScript
constructor(
  private msalService: MsalService,
  private alertsService: AlertsService) {

  this.authenticated = this.msalService.getUser() != null;
  this.getUser().then((user) => {this.user = user});
}
```

<span data-ttu-id="fcab1-150">Por último, quite el código temporal de `HomeComponent` la clase.</span><span class="sxs-lookup"><span data-stu-id="fcab1-150">Finally, remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="fcab1-151">Abra el `./src/app/home/home.component.ts` archivo y reemplace la función `signIn` existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="fcab1-151">Open the `./src/app/home/home.component.ts` file and replace the existing `signIn` function with the following.</span></span>

```TypeScript
async signIn(): Promise<void> {
  await this.authService.signIn();
}
```

<span data-ttu-id="fcab1-152">Ahora, si guarda los cambios e inicia la aplicación, después de iniciar sesión debe terminar de nuevo en la Página principal, pero la interfaz de usuario debe cambiar para indicar que ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="fcab1-152">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Una captura de pantalla de la Página principal después de iniciar sesión](./images/add-aad-auth-01.png)

<span data-ttu-id="fcab1-154">Haga clic en el avatar de usuario en la esquina superior derecha para acceder al vínculo **Cerrar sesión** .</span><span class="sxs-lookup"><span data-stu-id="fcab1-154">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="fcab1-155">Al hacer clic en **cerrar** sesión se restablece la sesión y se vuelve a la Página principal.</span><span class="sxs-lookup"><span data-stu-id="fcab1-155">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Captura de pantalla del menú desplegable con el vínculo cerrar sesión](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="fcab1-157">Almacenamiento y actualización de tokens</span><span class="sxs-lookup"><span data-stu-id="fcab1-157">Storing and refreshing tokens</span></span>

<span data-ttu-id="fcab1-158">En este punto, la aplicación tiene un token de acceso, que se envía `Authorization` en el encabezado de las llamadas a la API.</span><span class="sxs-lookup"><span data-stu-id="fcab1-158">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="fcab1-159">Este es el token que permite que la aplicación tenga acceso a Microsoft Graph en nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="fcab1-159">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="fcab1-160">Sin embargo, este token es de corta duración.</span><span class="sxs-lookup"><span data-stu-id="fcab1-160">However, this token is short-lived.</span></span> <span data-ttu-id="fcab1-161">El token expira una hora después de su emisión.</span><span class="sxs-lookup"><span data-stu-id="fcab1-161">The token expires an hour after it is issued.</span></span> <span data-ttu-id="fcab1-162">Debido a que la aplicación usa la biblioteca de MSAL, no tiene que implementar ninguna lógica de almacenamiento o actualización de tokens.</span><span class="sxs-lookup"><span data-stu-id="fcab1-162">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="fcab1-163">El `MsalService` almacena en caché el token en el almacenamiento del explorador.</span><span class="sxs-lookup"><span data-stu-id="fcab1-163">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="fcab1-164">El `acquireTokenSilent` método comprueba primero el token almacenado en caché y, si no lo ha expirado, lo devuelve.</span><span class="sxs-lookup"><span data-stu-id="fcab1-164">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="fcab1-165">Si ha expirado, realiza una solicitud silenciosa para obtener uno nuevo.</span><span class="sxs-lookup"><span data-stu-id="fcab1-165">If it is expired, it makes a silent request to obtain a new one.</span></span>