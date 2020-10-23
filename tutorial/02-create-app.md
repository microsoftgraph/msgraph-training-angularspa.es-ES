<!-- markdownlint-disable MD002 MD041 -->

En esta sección, creará un nuevo proyecto de angular.

1. Abra la interfaz de línea de comandos (CLI), vaya a un directorio donde tenga derechos para crear archivos y ejecute los siguientes comandos para instalar la herramienta [angular](https://www.npmjs.com/package/@angular/cli) de la CLI y crear una nueva aplicación de angular.

    ```Shell
    npm install -g @angular/cli@10.1.7
    ng new graph-tutorial
    ```

1. El CLI de angular le pedirá más información. Responda a los mensajes como se indica a continuación.

    ```Shell
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. Una vez que finalice el comando, cambie al `graph-tutorial` Directorio de la CLI y ejecute el siguiente comando para iniciar un servidor Web local.

    ```Shell
    ng serve --open
    ```

1. El explorador predeterminado se abre en [https://localhost:4200/](https://localhost:4200) con una página angular predeterminada. Si el explorador no se abre, ábralo y vaya a [https://localhost:4200/](https://localhost:4200) para comprobar que la nueva aplicación funciona.

## <a name="add-node-packages"></a>Agregar paquetes de nodo

Antes de continuar, instale algunos paquetes adicionales que usará más adelante:

- [bootstrap](https://github.com/twbs/bootstrap) para aplicar estilos y componentes comunes.
- [ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) para usar componentes bootstrap desde angular.
- [momento](https://github.com/moment/moment) para dar formato a fechas y horas.
- [Windows-IANA](https://github.com/rubenillodo/windows-iana)
- [msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) para autenticar en Azure Active Directory y recuperar tokens de acceso.
- [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.

1. Ejecute los siguientes comandos en su CLI.

    ```Shell
    npm install bootstrap@4.5.3 @ng-bootstrap/ng-bootstrap@7.0.0 msal@1.4.2 @azure/msal-angular@1.1.1
    npm install moment@2.29.1 moment-timezone@0.5.31 windows-iana@4.2.1
    npm install @microsoft/microsoft-graph-client@2.1.0 @microsoft/microsoft-graph-types@1.24.0
    ```

1. Ejecute el siguiente comando en su CLI para agregar el paquete de localización angular (necesario para ng-bootstrap).

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a>Diseñar la aplicación

En esta sección, creará la interfaz de usuario para la aplicación.

1. Abra **./src/Styles.CSS** y agregue las siguientes líneas.

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. Agregue el módulo bootstrap a la aplicación. Abra **./src/app/app.Module.ts** y reemplace su contenido por lo siguiente.

    ```typescript
    import { BrowserModule } from '@angular/platform-browser';
    import { FormsModule } from '@angular/forms';
    import { NgModule } from '@angular/core';
    import { NgbModule } from '@ng-bootstrap/ng-bootstrap';

    import { AppRoutingModule } from './app-routing.module';
    import { AppComponent } from './app.component';

    @NgModule({
      declarations: [
        AppComponent
      ],
      imports: [
        BrowserModule,
        FormsModule,
        AppRoutingModule,
        NgbModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule { }
    ```

1. Cree un nuevo archivo en la carpeta **./src/App** denominado **User. ts** y agregue el siguiente código.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. Genera un componente angular para la navegación superior en la página. En la CLI, ejecute el siguiente comando.

    ```Shell
    ng generate component nav-bar
    ```

1. Una vez que finalice el comando, Abra **./src/App/NAV-Bar/Nav-bar.Component.ts** y reemplace su contenido por lo siguiente.

    ```typescript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-nav-bar',
      templateUrl: './nav-bar.component.html',
      styleUrls: ['./nav-bar.component.css']
    })
    export class NavBarComponent implements OnInit {

      // Should the collapsed nav show?
      showNav: boolean;
      // Is a user logged in?
      authenticated: boolean;
      // The user
      user: User;

      constructor() { }

      ngOnInit() {
        this.showNav = false;
        this.authenticated = false;
        this.user = null;
      }

      // Used by the Bootstrap navbar-toggler button to hide/show
      // the nav in a collapsed state
      toggleNavBar(): void {
        this.showNav = !this.showNav;
      }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com',
          avatar: null
        };
      }

      signOut(): void {
        // Temporary
        this.authenticated = false;
        this.user = null;
      }
    }
    ```

1. Abra **./src/app/nav-bar/nav-bar.component.html** y reemplace su contenido por lo siguiente.

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. Cree una página principal para la aplicación. Ejecute el siguiente comando en su CLI.

    ```Shell
    ng generate component home
    ```

1. Una vez que finalice el comando, Abra **./src/App/Home/Home.Component.ts** y reemplace su contenido por lo siguiente.

    ```typescript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-home',
      templateUrl: './home.component.html',
      styleUrls: ['./home.component.css']
    })
    export class HomeComponent implements OnInit {

      // Is a user logged in?
      authenticated: boolean;
      // The user
      user: any;

      constructor() { }

      ngOnInit() {
        this.authenticated = false;
        this.user = {};
      }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com'
        };
      }
    }
    ```

1. Abra **./src/app/home/home.component.html** y reemplace su contenido por lo siguiente.

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. Cree una `Alert` clase simple. Cree un nuevo archivo en el directorio **./src/App** denominado **Alert. ts** y agregue el siguiente código.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. Cree un servicio de alerta que la aplicación pueda usar para mostrar mensajes al usuario. En la CLI, ejecute el siguiente comando.

    ```Shell
    ng generate service alerts
    ```

1. Abra **./src/App/alerts.Service.ts** y reemplace su contenido por lo siguiente.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. Generar un componente de alertas para mostrar alertas. En la CLI, ejecute el siguiente comando.

    ```Shell
    ng generate component alerts
    ```

1. Una vez que finalice el comando, Abra **./src/App/Alerts/alerts.Component.ts** y reemplace su contenido por lo siguiente.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. Abra **./src/app/alerts/alerts.component.html** y reemplace su contenido por lo siguiente.

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. Abra **./src/app/app-Routing.Module.ts** y reemplace la `const routes: Routes = [];` línea por el siguiente código.

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. Abra **./src/app/app.component.html** y reemplace todo su contenido por lo siguiente.

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

1. Agregue un archivo de imagen de su elección con el nombre **no-profile-photo.png** en el directorio **./src/assets** . Esta imagen se utilizará como foto del usuario cuando el usuario no tenga foto en Microsoft Graph.

Guarde todos los cambios y actualice la página. Ahora, la aplicación debe tener un aspecto muy diferente.

![Una captura de pantalla de la Página principal rediseñada](images/create-app-01.png)
