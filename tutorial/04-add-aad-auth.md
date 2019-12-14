<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD. Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph. En este paso, integrará la [biblioteca de autenticación de Microsoft para angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) en la aplicación.

Cree un nuevo archivo en el `./src` directorio denominado `oauth.ts` y agregue el siguiente código.

```TypeScript
export const OAuthSettings = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

Reemplace `YOUR_APP_ID_HERE` por el identificador de la aplicación del portal de registro de aplicaciones.

> [!IMPORTANT]
> Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el `oauth.ts` archivo del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación.

Abra `./src/app/app.module.ts` y agregue las siguientes `import` instrucciones en la parte superior del archivo.

```TypeScript
import { MsalModule } from '@azure/msal-angular';
import { OAuthSettings } from '../oauth';
```

A continuación, `MsalModule` agregue a `imports` la matriz dentro `@NgModule` de la declaración e inicialícela con el identificador de la aplicación.

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

## <a name="implement-sign-in"></a>Implementar el inicio de sesión

Empiece por definir una clase `User` sencilla para contener la información sobre el usuario que muestra la aplicación. Cree un nuevo archivo en la `./src/app` carpeta denominada `user.ts` y agregue el código siguiente.

```TypeScript
export class User {
  displayName: string;
  email: string;
  avatar: string;
}
```

Ahora cree un servicio de autenticación. Al crear un servicio para esto, puede inyectarlo fácilmente en cualquier componente que necesite acceso a los métodos de autenticación. Ejecute el siguiente comando en su CLI.

```Shell
ng generate service auth
```

Una vez que finalice el comando, `./src/app/auth.service.ts` Abra el archivo y reemplace el contenido por el código siguiente.

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

Ahora que tiene el servicio de autenticación, se puede inyectar en los componentes que inician sesión. Comience con el `NavBarComponent`. Abra el `./src/app/nav-bar/nav-bar.component.ts` archivo y realice los cambios siguientes.

- Agregar `import { AuthService } from '../auth.service';` a la parte superior del archivo.
- Quite las `authenticated` propiedades `user` y de la clase y quite el código que las establece en `ngOnInit`.
- Inserte el `AuthService` mediante la adición del siguiente parámetro a `constructor`: `private authService: AuthService`.
- Reemplace el método `signIn` existente con el siguiente:

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();
    }
    ```

- Reemplace el método `signOut` existente con el siguiente:

    ```TypeScript
    signOut(): void {
      this.authService.signOut();
    }
    ```

Cuando termine, el código debe tener un aspecto similar al siguiente.

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

Como ha quitado `authenticated` las `user` propiedades y de la clase, también tiene que actualizar el `./src/app/nav-bar/nav-bar.component.html` archivo. Abra el archivo y realice los cambios siguientes.

- Reemplace todas las instancias de `authenticated` por `authService.authenticated`.
- Reemplace todas las instancias `user` de `authService.user`por.

A continuación, `HomeComponent` actualice la clase. Realice todos los cambios que realizó en `./src/app/home/home.component.ts` la `NavBarComponent` clase con las siguientes excepciones.

- No hay ningún `signOut` método en la `HomeComponent` clase.
- Reemplace el `signIn` método por una versión ligeramente distinta. Este código llama `getAccessToken` a obtener un token de acceso, que, por ahora, generará el token como un error.

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();

      // Temporary to display the token
      if (this.authService.authenticated) {
        let token = await this.authService.getAccessToken();
      }
    }
    ```

Cuando termine, el archivo debe tener un aspecto similar al siguiente.

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

Por último, realice los mismos reemplazos en `./src/app/home/home.component.html` que usted realizó para la barra de navegación.

Guarde los cambios y actualice el explorador. Haga clic en el botón **haga clic aquí para iniciar sesión** y debe redirigirse a `https://login.microsoftonline.com`. Inicie sesión con su cuenta de Microsoft y dé su consentimiento a los permisos solicitados. La página de la aplicación debe actualizarse y mostrar el token.

### <a name="get-user-details"></a>Obtener detalles del usuario

Ahora, el servicio de autenticación establece valores constantes para el nombre para mostrar y la dirección de correo electrónico del usuario. Ahora que tiene un token de acceso, puede obtener detalles del usuario de Microsoft Graph para que esos valores se correspondan con el usuario actual. Abra `./src/app/auth.service.ts` y agregue la siguiente `import` instrucción a la parte superior del archivo.

```TypeScript
import { Client } from '@microsoft/microsoft-graph-client';
```

Agregue una nueva función llamada `AuthService` a la clase `getUser`.

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

Busque y quite el código siguiente en el `getAccessToken` método que agrega una alerta para mostrar el token de acceso.

```TypeScript
// Temporary to display token in an error box
if (result) this.alertsService.add('Token acquired', result);
```

Busque y quite el código siguiente del `signIn` método.

```TypeScript
// Temporary placeholder
this.user = new User();
this.user.displayName = "Adele Vance";
this.user.email = "AdeleV@contoso.com";
```

En su ubicación, agregue el siguiente código.

```TypeScript
this.user = await this.getUser();
```

Este nuevo código usa el SDK de Microsoft Graph para obtener los detalles del usuario y, a `User` continuación, crea un objeto con los valores devueltos por la llamada a la API.

Ahora, cambie `constructor` la para `AuthService` la clase para comprobar si el usuario ya ha iniciado sesión y cargar sus detalles en caso afirmativo. Reemplace el existente `constructor` por lo siguiente.

```TypeScript
constructor(
  private msalService: MsalService,
  private alertsService: AlertsService) {

  this.authenticated = this.msalService.getUser() != null;
  this.getUser().then((user) => {this.user = user});
}
```

Por último, quite el código temporal de `HomeComponent` la clase. Abra el `./src/app/home/home.component.ts` archivo y reemplace la función `signIn` existente por lo siguiente.

```TypeScript
async signIn(): Promise<void> {
  await this.authService.signIn();
}
```

Ahora, si guarda los cambios e inicia la aplicación, después de iniciar sesión debe terminar de nuevo en la Página principal, pero la interfaz de usuario debe cambiar para indicar que ha iniciado sesión.

![Una captura de pantalla de la Página principal después de iniciar sesión](./images/add-aad-auth-01.png)

Haga clic en el avatar de usuario en la esquina superior derecha para acceder al vínculo **Cerrar sesión** . Al hacer clic en **cerrar** sesión se restablece la sesión y se vuelve a la Página principal.

![Captura de pantalla del menú desplegable con el vínculo cerrar sesión](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Almacenamiento y actualización de tokens

En este punto, la aplicación tiene un token de acceso, que se envía `Authorization` en el encabezado de las llamadas a la API. Este es el token que permite que la aplicación tenga acceso a Microsoft Graph en nombre del usuario.

Sin embargo, este token es de corta duración. El token expira una hora después de su emisión. Debido a que la aplicación usa la biblioteca de MSAL, no tiene que implementar ninguna lógica de almacenamiento o actualización de tokens. El `MsalService` almacena en caché el token en el almacenamiento del explorador. El `acquireTokenSilent` método comprueba primero el token almacenado en caché y, si no lo ha expirado, lo devuelve. Si ha expirado, realiza una solicitud silenciosa para obtener uno nuevo.
