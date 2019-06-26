<!-- markdownlint-disable MD002 MD041 -->

Abra la interfaz de línea de comandos (CLI), vaya a un directorio donde tenga derechos para crear archivos y ejecute los siguientes comandos para instalar la herramienta angular de la [CLI](https://www.npmjs.com/package/@angular/cli) y crear una nueva aplicación de angular.

```Shell
npm install -g @angular/cli
ng new graph-tutorial
```

El CLI de angular le pedirá más información. Responda a los mensajes como se indica a continuación.

```Shell
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
```

Una vez que finalice el comando, cambie `graph-tutorial` al directorio de la CLI y ejecute el siguiente comando para iniciar un servidor Web local.

```Shell
ng serve --open
```

El explorador predeterminado se abre [https://localhost:4200/](https://localhost:4200) en con una página angular predeterminada. Si el explorador no se abre, ábralo y vaya [https://localhost:4200/](https://localhost:4200) a para comprobar que la nueva aplicación funciona.

Antes de continuar, instale algunos paquetes adicionales que usará más adelante:

- [bootstrap](https://github.com/twbs/bootstrap) para aplicar estilos y componentes comunes.
- [ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) para usar componentes bootstrap desde angular.
- [angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) para usar iconos de Fontawesome en angular.
- [fontawesome-SVG-Core](https://github.com/FortAwesome/Font-Awesome), [Free-regular-SVG-Icons](https://github.com/FortAwesome/Font-Awesome)y [Free-Solid-SVG-Icons](https://github.com/FortAwesome/Font-Awesome) para los iconos fontawesome usados en el ejemplo.
- [momento](https://github.com/moment/moment) para dar formato a fechas y horas.
- [msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) para autenticar en Azure Active Directory y recuperar tokens de acceso.
- [rxjs-compat](https://github.com/ReactiveX/rxjs/tree/master/compat), necesario para `msal-angular` el paquete.
- [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.

Ejecute el siguiente comando en su CLI.

```Shell
npm install bootstrap@4.3.1 @fortawesome/angular-fontawesome@0.3.0 @fortawesome/fontawesome-svg-core@1.2.17
npm install @fortawesome/free-regular-svg-icons@5.8.1 @fortawesome/free-solid-svg-icons@5.8.1
npm install moment@2.24.0 moment-timezone@0.5.25 @ng-bootstrap/ng-bootstrap@4.1.2
npm install @azure/msal-angular@0.1.2 rxjs-compat@6.5.1 @microsoft/microsoft-graph-client@1.6.0
```

## <a name="design-the-app"></a>Diseñar la aplicación

Empiece agregando los archivos CSS de bootstrap a la aplicación, así como algunos estilos globales. Abra el `./src/styles.css` y agregue las siguientes líneas.

```CSS
@import "~bootstrap/dist/css/bootstrap.css";

/* Add padding for the nav bar */
body {
  padding-top: 4.5rem;
}

/* Style debug info in alerts */
.alert-pre {
  word-wrap: break-word;
  word-break: break-all;
  white-space: pre-wrap;
}
```

A continuación, agregue los módulos bootstrap y FontAwesome a la aplicación. Abra `./src/app/app.module.ts` y agregue las siguientes `import` instrucciones en la parte superior del archivo.

```TypeScript
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
import { FontAwesomeModule } from '@fortawesome/angular-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
import { faUserCircle } from '@fortawesome/free-regular-svg-icons';
```

A continuación, agregue el siguiente código después de `import` todas las instrucciones.

```TypeScript
library.add(faExternalLinkAlt);
library.add(faUserCircle);
```

En la `@NgModule` declaración, reemplace la matriz `imports` existente por lo siguiente.

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule
]
```

Ahora, genere un componente angular para la navegación superior en la página. En la CLI, ejecute el siguiente comando.

```Shell
ng generate component nav-bar
```

Una vez que haya finalizado el comando `./src/app/nav-bar/nav-bar.component.ts` , abra el archivo y reemplace el contenido por lo siguiente.

```TypeScript
import { Component, OnInit } from '@angular/core';

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
  user: any;

  constructor() { }

  ngOnInit() {
    this.showNav = false;
    this.authenticated = false;
    this.user = {};
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
      email: 'AdeleV@contoso.com'
    };
  }

  signOut(): void {
    // Temporary
    this.authenticated = false;
    this.user = {};
  }
}
```

Abra el `./src/app/nav-bar/nav-bar.component.html` archivo y reemplace el contenido por lo siguiente.

```html
<nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
  <div class="container">
    <a routerLink="/" class="navbar-brand">Angular Graph Tutorial</a>
    <button class="navbar-toggler" type="button" (click)="toggleNavBar()" [attr.aria-expanded]="showNav"
    aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" [class.show]="showNav" id="navbarCollapse">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item">
          <a routerLink="/" class="nav-link" routerLinkActive="active">Home</a>
        </li>
        <li *ngIf="authenticated" class="nav-item">
          <a routerLink="/calendar" class="nav-link" routerLinkActive="active">Calendar</a>
        </li>
      </ul>
      <ul class="navbar-nav justify-content-end">
        <li class="nav-item">
          <a class="nav-link" href="https://docs.microsoft.com/graph/overview" target="_blank">
            <fa-icon [icon]="[ 'fas', 'external-link-alt' ]" class="mr-1"></fa-icon>Docs
          </a>
        </li>
        <li *ngIf="authenticated" ngbDropdown placement="bottom-right" class="nav-item">
          <a ngbDropdownToggle id="userMenu" class="nav-link" href="javascript:undefined" role="button" aria-haspopup="true"
            aria-expanded="false">
            <div *ngIf="user.avatar; then userAvatar else defaultAvatar"></div>
            <ng-template #userAvatar>
              <img src="user.avatar" class="rounded-circle align-self-center mr-2" style="width: 32px;">
            </ng-template>
            <ng-template #defaultAvatar>
              <fa-icon [icon]="[ 'far', 'user-circle' ]" fixedWidth="true" size="lg"
                class="rounded-circle align-self-center mr-2"></fa-icon>
            </ng-template>
          </a>
          <div ngbDropdownMenu aria-labelledby="userMenu">
            <h5 class="dropdown-item-text mb-0">{{user.displayName}}</h5>
            <p class="dropdown-item-text text-muted mb-0">{{user.email}}</p>
            <div class="dropdown-divider"></div>
            <a class="dropdown-item" href="javascript:undefined" role="button" (click)="signOut()">Sign Out</a>
          </div>
        </li>
        <li *ngIf="!authenticated" class="nav-item">
          <a class="nav-link" href="javascript:undefined" role="button" (click)="signIn()">Sign In</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

A continuación, cree una página principal para la aplicación. Ejecute el siguiente comando en su CLI.

```Shell
ng generate component home
```

Una vez que haya finalizado el comando `./src/app/home/home.component.ts` , abra el archivo y reemplace el contenido por lo siguiente.

```TypeScript
import { Component, OnInit } from '@angular/core';

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

A continuación, `./src/app/home/home.component.html` Abra el archivo y reemplace el contenido por lo siguiente.

```html
<div class="jumbotron">
  <h1>Angular Graph Tutorial</h1>
  <p class="lead">This sample app shows how to use the Microsoft Graph API from Angular</p>
  <div *ngIf="authenticated; then welcomeUser else signInPrompt"></div>
  <ng-template #welcomeUser>
    <h4>Welcome {{ user.displayName }}!</h4>
    <p>Use the navigation bar at the top of the page to get started.</p>
  </ng-template>
  <ng-template #signInPrompt>
    <a href="javascript:undefined" class="btn btn-primary btn-large" role="button" (click)="signIn()">Click here to sign in</a>
  </ng-template>
</div>
```

Ahora cree un servicio de alerta que la aplicación pueda usar para mostrar mensajes al usuario. Empiece por crear una clase `Alert` simple. Cree un nuevo archivo en el `./src/app` directorio denominado `alert.ts` y agregue el siguiente código.

```TypeScript
export class Alert {
  message: string;
  debug: string;
}
```

En la CLI, ejecute el siguiente comando.

```Shell
ng generate service alerts
```

Abra el `./src/app/alerts.service.ts` archivo y reemplace el contenido por lo siguiente.

```TypeScript
import { Injectable } from '@angular/core';
import { Alert } from './alert';

@Injectable({
  providedIn: 'root'
})
export class AlertsService {

  alerts: Alert[] = [];

  add(message: string, debug: string) {
    this.alerts.push({message: message, debug: debug});
  }

  remove(alert: Alert) {
    this.alerts.splice(this.alerts.indexOf(alert), 1);
  }
}
```

Ahora, genere un componente de alertas para mostrar alertas. En la CLI, ejecute el siguiente comando.

```Shell
ng generate component alerts
```

Una vez que haya finalizado el comando `./src/app/alerts/alerts.component.ts` , abra el archivo y reemplace el contenido por lo siguiente.

```TypeScript
import { Component, OnInit } from '@angular/core';
import { AlertsService } from '../alerts.service';
import { Alert } from '../alert';

@Component({
  selector: 'app-alerts',
  templateUrl: './alerts.component.html',
  styleUrls: ['./alerts.component.css']
})
export class AlertsComponent implements OnInit {

  constructor(private alertsService: AlertsService) { }

  ngOnInit() {
  }

  close(alert: Alert) {
    this.alertsService.remove(alert);
  }
}
```

A continuación, `./src/app/alerts/alerts.component.html` Abra el archivo y reemplace el contenido por lo siguiente.

```html
<div *ngFor="let alert of alertsService.alerts">
  <ngb-alert type="danger" (close)="close(alert)">
    <p>{{alert.message}}</p>
    <pre *ngIf="alert.debug" class="alert-pre border bg-light p-2"><code>{{alert.debug}}</code></pre>
  </ngb-alert>
</div>
```

Ahora con esos componentes básicos definidos, actualice la aplicación para usarlas. Primero, abra el `./src/app/app-routing.module.ts` archivo y reemplace la `const routes: Routes = [];` línea con el siguiente código.

```TypeScript
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
];
```

Abra el archivo `./src/app/app.component.html` y reemplace todo su contenido por lo siguiente.

```html
<app-nav-bar></app-nav-bar>
<main role="main" class="container">
  <app-alerts></app-alerts>
  <router-outlet></router-outlet>
</main>
```

Guarde todos los cambios y actualice la página. Ahora, la aplicación debe tener un aspecto muy diferente.

![Una captura de pantalla de la Página principal rediseñada](images/create-app-01.png)
