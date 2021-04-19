<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f2f22-101">En esta sección, creará un nuevo proyecto angular.</span><span class="sxs-lookup"><span data-stu-id="f2f22-101">In this section, you'll create a new Angular project.</span></span>

1. <span data-ttu-id="f2f22-102">Abra la interfaz de línea de comandos (CLI), vaya a un directorio donde tenga derechos para crear archivos y ejecute los siguientes comandos para instalar la herramienta [de la CLI](https://www.npmjs.com/package/@angular/cli) de Angular y crear una nueva aplicación angular.</span><span class="sxs-lookup"><span data-stu-id="f2f22-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

    ```Shell
    npm install -g @angular/cli@11.2.9
    ng new graph-tutorial
    ```

1. <span data-ttu-id="f2f22-103">La CLI de Angular pedirá más información.</span><span class="sxs-lookup"><span data-stu-id="f2f22-103">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="f2f22-104">Responda a los mensajes de la siguiente manera.</span><span class="sxs-lookup"><span data-stu-id="f2f22-104">Answer the prompts as follows.</span></span>

    ```Shell
    ? Do you want to enforce stricter type checking and stricter bundle budgets in the workspace? Yes
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. <span data-ttu-id="f2f22-105">Cuando finalice el comando, cambie al directorio de la CLI y `graph-tutorial` ejecute el siguiente comando para iniciar un servidor web local.</span><span class="sxs-lookup"><span data-stu-id="f2f22-105">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    ng serve --open
    ```

1. <span data-ttu-id="f2f22-106">El explorador predeterminado se abre con [https://localhost:4200/](https://localhost:4200) una página Angular predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f2f22-106">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="f2f22-107">Si el explorador no se abre, ábralo y busque [https://localhost:4200/](https://localhost:4200) para comprobar que la nueva aplicación funciona.</span><span class="sxs-lookup"><span data-stu-id="f2f22-107">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="f2f22-108">Agregar paquetes de nodo</span><span class="sxs-lookup"><span data-stu-id="f2f22-108">Add Node packages</span></span>

<span data-ttu-id="f2f22-109">Antes de seguir adelante, instale algunos paquetes adicionales que usará más adelante:</span><span class="sxs-lookup"><span data-stu-id="f2f22-109">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="f2f22-110">[bootstrap](https://github.com/twbs/bootstrap) para el estilo y componentes comunes.</span><span class="sxs-lookup"><span data-stu-id="f2f22-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="f2f22-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) para usar componentes de Bootstrap desde Angular.</span><span class="sxs-lookup"><span data-stu-id="f2f22-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="f2f22-112">[momento](https://github.com/moment/moment) para dar formato a fechas y horas.</span><span class="sxs-lookup"><span data-stu-id="f2f22-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- [<span data-ttu-id="f2f22-113">windows-iana</span><span class="sxs-lookup"><span data-stu-id="f2f22-113">windows-iana</span></span>](https://github.com/rubenillodo/windows-iana)
- <span data-ttu-id="f2f22-114">[msal-angular para](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) autenticarse en Azure Active Directory y recuperar tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="f2f22-114">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="f2f22-115">[microsoft-graph-client para](https://github.com/microsoftgraph/msgraph-sdk-javascript) realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="f2f22-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="f2f22-116">Ejecute los siguientes comandos en la CLI.</span><span class="sxs-lookup"><span data-stu-id="f2f22-116">Run the following commands in your CLI.</span></span>

    ```Shell
    npm install bootstrap@4.6.0 @ng-bootstrap/ng-bootstrap@9.1.0
    npm install @azure/msal-browser@2.14.0 @azure/msal-angular@2.0.0-beta.4
    npm install moment-timezone@0.5.33 windows-iana@5.0.1
    npm install @microsoft/microsoft-graph-client@2.2.1 @microsoft/microsoft-graph-types@1.35.0
    ```

1. <span data-ttu-id="f2f22-117">Ejecute el siguiente comando en la CLI para agregar el paquete de localización angular (requerido por ng-bootstrap).</span><span class="sxs-lookup"><span data-stu-id="f2f22-117">Run the following command in your CLI to add the Angular localization package (required by ng-bootstrap).</span></span>

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a><span data-ttu-id="f2f22-118">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="f2f22-118">Design the app</span></span>

<span data-ttu-id="f2f22-119">En esta sección, crearás la interfaz de usuario para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f2f22-119">In this section you'll create the user interface for the app.</span></span>

1. <span data-ttu-id="f2f22-120">Abra **./src/styles.css** y agregue las siguientes líneas.</span><span class="sxs-lookup"><span data-stu-id="f2f22-120">Open **./src/styles.css** and add the following lines.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. <span data-ttu-id="f2f22-121">Agrega el módulo Bootstrap a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f2f22-121">Add the Bootstrap module to the app.</span></span> <span data-ttu-id="f2f22-122">Abre **./src/app/app.module.ts** y reemplaza su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="f2f22-122">Open **./src/app/app.module.ts** and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="f2f22-123">Cree un nuevo archivo en la **carpeta ./src/app** denominada **user.ts** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="f2f22-123">Create a new file in the **./src/app** folder named **user.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="UserSnippet":::

1. <span data-ttu-id="f2f22-124">Genere un componente Angular para la navegación superior de la página.</span><span class="sxs-lookup"><span data-stu-id="f2f22-124">Generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="f2f22-125">En la CLI, ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="f2f22-125">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component nav-bar
    ```

1. <span data-ttu-id="f2f22-126">Una vez completado el comando, abra **./src/app/nav-bar/nav-bar.component.ts** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="f2f22-126">Once the command completes, open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="f2f22-127">Abra **./src/app/nav-bar/nav-bar.component.html** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="f2f22-127">Open **./src/app/nav-bar/nav-bar.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. <span data-ttu-id="f2f22-128">Crea una página principal para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f2f22-128">Create a home page for the app.</span></span> <span data-ttu-id="f2f22-129">Ejecute el siguiente comando en la CLI.</span><span class="sxs-lookup"><span data-stu-id="f2f22-129">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component home
    ```

1. <span data-ttu-id="f2f22-130">Una vez completado el comando, abra **./src/app/home/home.component.ts** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="f2f22-130">Once the command completes, open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="f2f22-131">Abra **./src/app/home/home.component.html** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="f2f22-131">Open **./src/app/home/home.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. <span data-ttu-id="f2f22-132">Cree una clase `Alert` sencilla.</span><span class="sxs-lookup"><span data-stu-id="f2f22-132">Create a simple `Alert` class.</span></span> <span data-ttu-id="f2f22-133">Cree un nuevo archivo en **el directorio ./src/app** denominado **alert.ts** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="f2f22-133">Create a new file in the **./src/app** directory named **alert.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="AlertSnippet":::

1. <span data-ttu-id="f2f22-134">Crea un servicio de alertas que la aplicación puede usar para mostrar mensajes al usuario.</span><span class="sxs-lookup"><span data-stu-id="f2f22-134">Create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="f2f22-135">En la CLI, ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="f2f22-135">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate service alerts
    ```

1. <span data-ttu-id="f2f22-136">Abra **./src/app/alerts.service.ts** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="f2f22-136">Open **./src/app/alerts.service.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. <span data-ttu-id="f2f22-137">Generar un componente de alertas para mostrar alertas.</span><span class="sxs-lookup"><span data-stu-id="f2f22-137">Generate an alerts component to display alerts.</span></span> <span data-ttu-id="f2f22-138">En la CLI, ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="f2f22-138">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component alerts
    ```

1. <span data-ttu-id="f2f22-139">Una vez completado el comando, abra **./src/app/alerts/alerts.component.ts** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="f2f22-139">Once the command completes, open **./src/app/alerts/alerts.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="AlertsComponentSnippet":::

1. <span data-ttu-id="f2f22-140">Abra **./src/app/alerts/alerts.component.html** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="f2f22-140">Open **./src/app/alerts/alerts.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="AlertHtml":::

1. <span data-ttu-id="f2f22-141">Abra **./src/app/app-routing.module.ts** y reemplace la `const routes: Routes = [];` línea por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="f2f22-141">Open **./src/app/app-routing.module.ts** and replace the `const routes: Routes = [];` line with the following code.</span></span>

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. <span data-ttu-id="f2f22-142">Abra **./src/app/app.component.html** y reemplace todo su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="f2f22-142">Open **./src/app/app.component.html** and replace its entire contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="AppHtml":::

1. <span data-ttu-id="f2f22-143">Agregue un archivo de imagen de su elección denominado **no-profile-photo.png** en **el directorio ./src/assets.**</span><span class="sxs-lookup"><span data-stu-id="f2f22-143">Add an image file of your choosing named **no-profile-photo.png** in the **./src/assets** directory.</span></span> <span data-ttu-id="f2f22-144">Esta imagen se usará como la foto del usuario cuando el usuario no tenga ninguna foto en Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="f2f22-144">This image will be used as the user's photo when the user has no photo in Microsoft Graph.</span></span>

<span data-ttu-id="f2f22-145">Guarde todos los cambios y actualice la página.</span><span class="sxs-lookup"><span data-stu-id="f2f22-145">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="f2f22-146">Ahora, la aplicación debe tener un aspecto muy diferente.</span><span class="sxs-lookup"><span data-stu-id="f2f22-146">Now, the app should look very different.</span></span>

![Una captura de pantalla de la página de inicio rediseñada](images/create-app-01.png)
