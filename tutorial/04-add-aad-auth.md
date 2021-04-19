<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, extenderá la aplicación desde el ejercicio anterior para admitir la autenticación con Azure AD. Esto es necesario para obtener el token de acceso OAuth necesario para llamar a Microsoft Graph. En este paso, integrará la Biblioteca de [autenticación](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) de Microsoft para Angular en la aplicación.

1. Cree un nuevo archivo en el directorio **./src** denominado **oauth.ts** y agregue el siguiente código.

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.example.ts":::

    Reemplace `YOUR_APP_ID_HERE` por el identificador de aplicación del Portal de registro de aplicaciones.

    > [!IMPORTANT]
    > Si usas el control de código fuente como git, ahora sería un buen momento para excluir el archivo **oauth.ts** del control de código fuente para evitar la pérdida involuntaria del identificador de la aplicación.

1. Abre **./src/app/app.module.ts** y agrega las siguientes `import` instrucciones a la parte superior del archivo.

    ```typescript
    import { IPublicClientApplication,
             PublicClientApplication,
             BrowserCacheLocation } from '@azure/msal-browser';
    import { MsalModule,
             MsalService,
             MSAL_INSTANCE } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. Agregue la siguiente función debajo `import` de las instrucciones.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="MSALFactorySnippet":::

1. Agregue la `MsalModule` matriz `imports` dentro de la `@NgModule` declaración.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ImportsSnippet" highlight="6":::

1. Agregue el `MSALInstanceFactory` y `MsalService` a la `providers` matriz dentro de la `@NgModule` declaración.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ProvidersSnippet" highlight="2-6":::

## <a name="implement-sign-in"></a>Implementar el inicio de sesión

En esta sección, creará un servicio de autenticación e implementará el inicio de sesión y la salida.

1. Ejecute el siguiente comando en la CLI.

    ```Shell
    ng generate service auth
    ```

    Al crear un servicio para esto, puede insertarlo fácilmente en cualquier componente que necesite acceso a los métodos de autenticación.

1. Una vez que finalice el comando, abra **./src/app/auth.service.ts** y reemplace su contenido por el código siguiente.

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

1. Abra **./src/app/nav-bar/nav-bar.component.ts** y reemplace su contenido por lo siguiente.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,34-36,38-40":::

1. Abra **./src/app/home/home.component.ts** y reemplace su contenido por lo siguiente.

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,13-20,22,26-33":::

Guarde los cambios y actualice el explorador. Haga clic **en el botón Hacer clic aquí para iniciar sesión** y se le redirigirá a `https://login.microsoftonline.com` . Inicie sesión con su cuenta de Microsoft y consiente los permisos solicitados. La página de la aplicación debe actualizarse y mostrar el token.

### <a name="get-user-details"></a>Obtener detalles del usuario

En este momento, el servicio de autenticación establece valores constantes para el nombre para mostrar y la dirección de correo electrónico del usuario. Ahora que tiene un token de acceso, puede obtener detalles de usuario de Microsoft Graph para que esos valores correspondan al usuario actual.

1. Abra **./src/app/auth.service.ts** y agregue las siguientes `import` instrucciones a la parte superior del archivo.

    ```typescript
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    ```

1. Agregue una nueva función llamada `AuthService` a la clase `getUser`.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="GetUserSnippet":::

1. Busque y quite el siguiente código en el `getAccessToken` método que agrega una alerta para mostrar el token de acceso.

    ```typescript
    // Temporary to display token in an error box
    this.alertsService.addSuccess('Token acquired', result);
    ```

1. Busque y quite el siguiente código del `signIn` método.

    ```typescript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    this.user.avatar = '/assets/no-profile-photo.png';
    ```

1. En su lugar, agregue el siguiente código.

    ```typescript
    this.user = await this.getUser();
    ```

    Este nuevo código usa el SDK de Microsoft Graph para obtener los detalles del usuario y, a continuación, crea un objeto con valores devueltos `User` por la llamada a la API.

1. Cambie la clase para comprobar si el usuario ya ha iniciado sesión y `constructor` cargue sus detalles si es `AuthService` así. Reemplace el existente `constructor` por lo siguiente.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="ConstructorSnippet" highlight="5-7":::

1. Quite el código temporal de la `HomeComponent` clase. Abra **./src/app/home/home.component.ts** y reemplace la función `signIn` existente por la siguiente.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="SignInSnippet":::

Ahora, si guarda los cambios e inicia la aplicación, después del inicio de sesión debe volver a la página principal, pero la interfaz de usuario debe cambiar para indicar que ha iniciado sesión.

![Una captura de pantalla de la página de inicio después de iniciar sesión](./images/add-aad-auth-01.png)

Haga clic en el avatar del usuario en la esquina superior derecha para obtener acceso al vínculo **Cerrar sesión.** Al **hacer clic en** Cerrar sesión, se restablece la sesión y se devuelve a la página principal.

![Una captura de pantalla del menú desplegable con el vínculo Cerrar sesión](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Almacenar y actualizar tokens

En este momento, la aplicación tiene un token de acceso, que se envía en el `Authorization` encabezado de las llamadas API. Este es el token que permite a la aplicación tener acceso a Microsoft Graph en nombre del usuario.

Sin embargo, este token es de corta duración. El token expira una hora después de su emisión. Dado que la aplicación usa la biblioteca MSAL, no es necesario implementar ninguna lógica de actualización o almacenamiento de tokens. El `MsalService` token se almacena en caché en el almacenamiento del explorador. El método primero comprueba el token almacenado en caché y, si no ha `acquireTokenSilent` expirado, lo devuelve. Si ha expirado, realiza una solicitud silenciosa para obtener una nueva.
