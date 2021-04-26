<!-- markdownlint-disable MD002 MD041 -->

En esta sección, creará un nuevo proyecto Angular proyecto.

1. Abra la interfaz de línea de comandos (CLI), vaya a un directorio donde tenga derechos para crear archivos y ejecute los siguientes comandos para instalar la herramienta [de cli](https://www.npmjs.com/package/@angular/cli) de Angular y crear una nueva aplicación Angular.

    ```Shell
    npm install -g @angular/cli@11.2.9
    ng new graph-tutorial
    ```

1. La Angular CLI pedirá más información. Responda a los mensajes de la siguiente manera.

    ```Shell
    ? Do you want to enforce stricter type checking and stricter bundle budgets in the workspace? Yes
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. Cuando finalice el comando, cambie al directorio de la CLI y `graph-tutorial` ejecute el siguiente comando para iniciar un servidor web local.

    ```Shell
    ng serve --open
    ```

1. El explorador predeterminado se abre [https://localhost:4200/](https://localhost:4200) con una página Angular predeterminada. Si el explorador no se abre, ábralo y busque [https://localhost:4200/](https://localhost:4200) para comprobar que la nueva aplicación funciona.

## <a name="add-node-packages"></a>Agregar paquetes de nodo

Antes de seguir adelante, instale algunos paquetes adicionales que usará más adelante:

- [bootstrap](https://github.com/twbs/bootstrap) para el estilo y componentes comunes.
- [ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) para usar componentes de Bootstrap desde Angular.
- [momento](https://github.com/moment/moment) para dar formato a fechas y horas.
- [windows-iana](https://github.com/rubenillodo/windows-iana)
- [msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) para autenticar a Azure Active Directory y recuperar tokens de acceso.
- [microsoft-graph-client para](https://github.com/microsoftgraph/msgraph-sdk-javascript) realizar llamadas a Microsoft Graph.

1. Ejecute los siguientes comandos en la CLI.

    ```Shell
    npm install bootstrap@4.6.0 @ng-bootstrap/ng-bootstrap@9.1.0
    npm install @azure/msal-browser@2.14.0 @azure/msal-angular@2.0.0-beta.4
    npm install moment-timezone@0.5.33 windows-iana@5.0.2
    npm install @microsoft/microsoft-graph-client@2.2.1 @microsoft/microsoft-graph-types@1.35.0
    ```

1. Ejecute el siguiente comando en la CLI para agregar el Angular de localización (requerido por ng-bootstrap).

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a>Diseñar la aplicación

En esta sección, crearás la interfaz de usuario para la aplicación.

1. Abra **./src/styles.css** y agregue las siguientes líneas.

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. Agrega el módulo Bootstrap a la aplicación. Abre **./src/app/app.module.ts** y reemplaza su contenido por lo siguiente.

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

1. Cree un nuevo archivo en la **carpeta ./src/app** denominada **user.ts** y agregue el siguiente código.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="UserSnippet":::

1. Genere un Angular para la navegación superior de la página. En la CLI, ejecute el siguiente comando.

    ```Shell
    ng generate component nav-bar
    ```

1. Una vez completado el comando, abra **./src/app/nav-bar/nav-bar.component.ts** y reemplace su contenido por lo siguiente.

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
      showNav: boolean = false;
      // Is a user logged in?
      authenticated: boolean = false;
      // The user
      user?: User = undefined;

      constructor() { }

      ngOnInit() { }

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
          avatar: '',
          timeZone: ''
        };
      }

      signOut(): void {
        // Temporary
        this.authenticated = false;
        this.user = undefined;
      }
    }
    ```

1. Abra **./src/app/nav-bar/nav-bar.component.html** y reemplace su contenido por lo siguiente.

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. Crea una página principal para la aplicación. Ejecute el siguiente comando en la CLI.

    ```Shell
    ng generate component home
    ```

1. Una vez completado el comando, abra **./src/app/home/home.component.ts** y reemplace su contenido por lo siguiente.

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
      authenticated: boolean = false;
      // The user
      user?: User = undefined;

      constructor() { }

      ngOnInit() { }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com',
          avatar: '',
          timeZone: ''
        };
      }
    }
    ```

1. Abra **./src/app/home/home.component.html** y reemplace su contenido por lo siguiente.

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. Cree una clase `Alert` sencilla. Cree un nuevo archivo en **el directorio ./src/app** denominado **alert.ts** y agregue el siguiente código.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="AlertSnippet":::

1. Crea un servicio de alertas que la aplicación puede usar para mostrar mensajes al usuario. En la CLI, ejecute el siguiente comando.

    ```Shell
    ng generate service alerts
    ```

1. Abra **./src/app/alerts.service.ts** y reemplace su contenido por lo siguiente.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. Generar un componente de alertas para mostrar alertas. En la CLI, ejecute el siguiente comando.

    ```Shell
    ng generate component alerts
    ```

1. Una vez completado el comando, abra **./src/app/alerts/alerts.component.ts** y reemplace su contenido por lo siguiente.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="AlertsComponentSnippet":::

1. Abra **./src/app/alerts/alerts.component.html** y reemplace su contenido por lo siguiente.

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="AlertHtml":::

1. Abra **./src/app/app-routing.module.ts** y reemplace la `const routes: Routes = [];` línea por el código siguiente.

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. Abra **./src/app/app.component.html** y reemplace todo su contenido por lo siguiente.

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="AppHtml":::

1. Agregue un archivo de imagen de su elección denominado **no-profile-photo.png** en **el directorio ./src/assets.** Esta imagen se usará como foto del usuario cuando el usuario no tenga ninguna foto en Microsoft Graph.

Guarde todos los cambios y actualice la página. Ahora, la aplicación debe tener un aspecto muy diferente.

![Una captura de pantalla de la página de inicio rediseñada](images/create-app-01.png)
