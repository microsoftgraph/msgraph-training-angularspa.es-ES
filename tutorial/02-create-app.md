<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="84e5a-101">En esta sección, creará un nuevo proyecto de angular.</span><span class="sxs-lookup"><span data-stu-id="84e5a-101">In this section, you'll create a new Angular project.</span></span>

1. <span data-ttu-id="84e5a-102">Abra la interfaz de línea de comandos (CLI), vaya a un directorio donde tenga derechos para crear archivos y ejecute los siguientes comandos para instalar la herramienta [angular](https://www.npmjs.com/package/@angular/cli) de la CLI y crear una nueva aplicación de angular.</span><span class="sxs-lookup"><span data-stu-id="84e5a-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

    ```Shell
    npm install -g @angular/cli@9.0.6
    ng new graph-tutorial
    ```

1. <span data-ttu-id="84e5a-103">El CLI de angular le pedirá más información.</span><span class="sxs-lookup"><span data-stu-id="84e5a-103">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="84e5a-104">Responda a los mensajes como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="84e5a-104">Answer the prompts as follows.</span></span>

    ```Shell
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. <span data-ttu-id="84e5a-105">Una vez que finalice el comando, cambie `graph-tutorial` al directorio de la CLI y ejecute el siguiente comando para iniciar un servidor Web local.</span><span class="sxs-lookup"><span data-stu-id="84e5a-105">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    ng serve --open
    ```

1. <span data-ttu-id="84e5a-106">El explorador predeterminado se abre [https://localhost:4200/](https://localhost:4200) en con una página angular predeterminada.</span><span class="sxs-lookup"><span data-stu-id="84e5a-106">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="84e5a-107">Si el explorador no se abre, ábralo y vaya [https://localhost:4200/](https://localhost:4200) a para comprobar que la nueva aplicación funciona.</span><span class="sxs-lookup"><span data-stu-id="84e5a-107">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="84e5a-108">Agregar paquetes de nodo</span><span class="sxs-lookup"><span data-stu-id="84e5a-108">Add Node packages</span></span>

<span data-ttu-id="84e5a-109">Antes de continuar, instale algunos paquetes adicionales que usará más adelante:</span><span class="sxs-lookup"><span data-stu-id="84e5a-109">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="84e5a-110">[bootstrap](https://github.com/twbs/bootstrap) para aplicar estilos y componentes comunes.</span><span class="sxs-lookup"><span data-stu-id="84e5a-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="84e5a-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) para usar componentes bootstrap desde angular.</span><span class="sxs-lookup"><span data-stu-id="84e5a-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="84e5a-112">[angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) para usar iconos de Fontawesome en angular.</span><span class="sxs-lookup"><span data-stu-id="84e5a-112">[angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) to use FontAwesome icons in Angular.</span></span>
- <span data-ttu-id="84e5a-113">[fontawesome-SVG-Core](https://github.com/FortAwesome/Font-Awesome), [Free-regular-SVG-Icons](https://github.com/FortAwesome/Font-Awesome)y [Free-Solid-SVG-Icons](https://github.com/FortAwesome/Font-Awesome) para los iconos fontawesome usados en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="84e5a-113">[fontawesome-svg-core](https://github.com/FortAwesome/Font-Awesome), [free-regular-svg-icons](https://github.com/FortAwesome/Font-Awesome), and [free-solid-svg-icons](https://github.com/FortAwesome/Font-Awesome) for the FontAwesome icons used in the sample.</span></span>
- <span data-ttu-id="84e5a-114">[momento](https://github.com/moment/moment) para dar formato a fechas y horas.</span><span class="sxs-lookup"><span data-stu-id="84e5a-114">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="84e5a-115">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) para autenticar en Azure Active Directory y recuperar tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="84e5a-115">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="84e5a-116">[Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="84e5a-116">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="84e5a-117">Ejecute los siguientes comandos en su CLI.</span><span class="sxs-lookup"><span data-stu-id="84e5a-117">Run the following commands in your CLI.</span></span>

    ```Shell
    npm install bootstrap@4.4.1 @fortawesome/angular-fontawesome@0.6.0 @fortawesome/fontawesome-svg-core@1.2.27
    npm install @fortawesome/free-regular-svg-icons@5.12.1 @fortawesome/free-solid-svg-icons@5.12.1
    npm install moment@2.24.0 moment-timezone@0.5.28 @ng-bootstrap/ng-bootstrap@6.0.0
    npm install msal@1.2.1 @azure/msal-angular@1.0.0-beta.4 @microsoft/microsoft-graph-client@2.0.0
    ```

1. <span data-ttu-id="84e5a-118">Ejecute el siguiente comando en su CLI para agregar el paquete de localización angular (necesario para ng-bootstrap).</span><span class="sxs-lookup"><span data-stu-id="84e5a-118">Run the following command in your CLI to add the Angular localization package (required by ng-bootstrap).</span></span>

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a><span data-ttu-id="84e5a-119">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="84e5a-119">Design the app</span></span>

<span data-ttu-id="84e5a-120">En esta sección, creará la interfaz de usuario para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="84e5a-120">In this section you'll create the user interface for the app.</span></span>

1. <span data-ttu-id="84e5a-121">Abra el `./src/styles.css` y agregue las siguientes líneas.</span><span class="sxs-lookup"><span data-stu-id="84e5a-121">Open the `./src/styles.css` and add the following lines.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. <span data-ttu-id="84e5a-122">Agregue los módulos bootstrap y FontAwesome a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="84e5a-122">Add the Bootstrap and FontAwesome modules to the app.</span></span> <span data-ttu-id="84e5a-123">Abra `./src/app/app.module.ts` y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="84e5a-123">Open `./src/app/app.module.ts` and replace its contents with the following.</span></span>

    ```TypeScript
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
    import { FontAwesomeModule, FaIconLibrary } from '@fortawesome/angular-fontawesome';
    import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
    import { faUserCircle } from '@fortawesome/free-regular-svg-icons';

    import { AppRoutingModule } from './app-routing.module';
    import { AppComponent } from './app.component';
    import { NavBarComponent } from './nav-bar/nav-bar.component';
    import { HomeComponent } from './home/home.component';
    import { AlertsComponent } from './alerts/alerts.component';

    @NgModule({
      declarations: [
        AppComponent,
        NavBarComponent,
        HomeComponent,
        AlertsComponent
      ],
      imports: [
        BrowserModule,
        AppRoutingModule,
        NgbModule,
        FontAwesomeModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule {
      constructor(library: FaIconLibrary) {
        // Register the FontAwesome icons used by the app
        library.addIcons(faExternalLinkAlt, faUserCircle);
      }
     }
    ```

1. <span data-ttu-id="84e5a-124">Cree un nuevo archivo en la `./src/app` carpeta denominada `user.ts` y agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="84e5a-124">Create a new file in the `./src/app` folder named `user.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. <span data-ttu-id="84e5a-125">Genera un componente angular para la navegación superior en la página.</span><span class="sxs-lookup"><span data-stu-id="84e5a-125">Generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="84e5a-126">En la CLI, ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="84e5a-126">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component nav-bar
    ```

1. <span data-ttu-id="84e5a-127">Una vez que haya finalizado el comando `./src/app/nav-bar/nav-bar.component.ts` , abra el archivo y reemplace el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="84e5a-127">Once the command completes, open the `./src/app/nav-bar/nav-bar.component.ts` file and replace its contents with the following.</span></span>

    ```TypeScript
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

1. <span data-ttu-id="84e5a-128">Abra el `./src/app/nav-bar/nav-bar.component.html` archivo y reemplace el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="84e5a-128">Open the `./src/app/nav-bar/nav-bar.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. <span data-ttu-id="84e5a-129">Cree una página principal para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="84e5a-129">Create a home page for the app.</span></span> <span data-ttu-id="84e5a-130">Ejecute el siguiente comando en su CLI.</span><span class="sxs-lookup"><span data-stu-id="84e5a-130">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component home
    ```

1. <span data-ttu-id="84e5a-131">Una vez que haya finalizado el comando `./src/app/home/home.component.ts` , abra el archivo y reemplace el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="84e5a-131">Once the command completes, open the `./src/app/home/home.component.ts` file and replace its contents with the following.</span></span>

    ```TypeScript
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

1. <span data-ttu-id="84e5a-132">Abra el `./src/app/home/home.component.html` archivo y reemplace el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="84e5a-132">Open the `./src/app/home/home.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. <span data-ttu-id="84e5a-133">Cree una clase `Alert` simple.</span><span class="sxs-lookup"><span data-stu-id="84e5a-133">Create a simple `Alert` class.</span></span> <span data-ttu-id="84e5a-134">Cree un nuevo archivo en el `./src/app` directorio denominado `alert.ts` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="84e5a-134">Create a new file in the `./src/app` directory named `alert.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. <span data-ttu-id="84e5a-135">Cree un servicio de alerta que la aplicación pueda usar para mostrar mensajes al usuario.</span><span class="sxs-lookup"><span data-stu-id="84e5a-135">Create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="84e5a-136">En la CLI, ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="84e5a-136">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate service alerts
    ```

1. <span data-ttu-id="84e5a-137">Abra el `./src/app/alerts.service.ts` archivo y reemplace el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="84e5a-137">Open the `./src/app/alerts.service.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. <span data-ttu-id="84e5a-138">Generar un componente de alertas para mostrar alertas.</span><span class="sxs-lookup"><span data-stu-id="84e5a-138">Generate an alerts component to display alerts.</span></span> <span data-ttu-id="84e5a-139">En la CLI, ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="84e5a-139">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component alerts
    ```

1. <span data-ttu-id="84e5a-140">Una vez que haya finalizado el comando `./src/app/alerts/alerts.component.ts` , abra el archivo y reemplace el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="84e5a-140">Once the command completes, open the `./src/app/alerts/alerts.component.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. <span data-ttu-id="84e5a-141">Abra el `./src/app/alerts/alerts.component.html` archivo y reemplace el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="84e5a-141">Open the `./src/app/alerts/alerts.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. <span data-ttu-id="84e5a-142">Abra el `./src/app/app-routing.module.ts` archivo y reemplace la `const routes: Routes = [];` línea con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="84e5a-142">Open the `./src/app/app-routing.module.ts` file and replace the `const routes: Routes = [];` line with the following code.</span></span>

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. <span data-ttu-id="84e5a-143">Abra el archivo `./src/app/app.component.html` y reemplace todo su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="84e5a-143">Open the `./src/app/app.component.html` file and replace its entire contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

<span data-ttu-id="84e5a-144">Guarde todos los cambios y actualice la página.</span><span class="sxs-lookup"><span data-stu-id="84e5a-144">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="84e5a-145">Ahora, la aplicación debe tener un aspecto muy diferente.</span><span class="sxs-lookup"><span data-stu-id="84e5a-145">Now, the app should look very different.</span></span>

![Una captura de pantalla de la Página principal rediseñada](images/create-app-01.png)
