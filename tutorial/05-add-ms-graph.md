<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4725c-101">En este ejercicio, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4725c-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="4725c-102">Para esta aplicación, usará la biblioteca de [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="4725c-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="4725c-103">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="4725c-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="4725c-104">Agregue un nuevo servicio para que contenga todas las llamadas de gráfico.</span><span class="sxs-lookup"><span data-stu-id="4725c-104">Add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="4725c-105">Ejecute el siguiente comando en su CLI.</span><span class="sxs-lookup"><span data-stu-id="4725c-105">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service graph
    ```

    <span data-ttu-id="4725c-106">Al igual que con el servicio de autenticación que creó anteriormente, la creación de un servicio le permite inyectarlo en cualquier componente que necesite acceso a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="4725c-106">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span>

1. <span data-ttu-id="4725c-107">Una vez que finalice el comando, Abra **./src/App/Graph.Service.ts** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="4725c-107">Once the command completes, open **./src/app/graph.service.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Injectable } from '@angular/core';
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';

    import { AuthService } from './auth.service';
    import { AlertsService } from './alerts.service';

    @Injectable({
      providedIn: 'root'
    })

    export class GraphService {

      private graphClient: Client;
      constructor(
        private authService: AuthService,
        private alertsService: AlertsService) {

        // Initialize the Graph client
        this.graphClient = Client.init({
          authProvider: async (done) => {
            // Get the token from the auth service
            let token = await this.authService.getAccessToken()
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
      }

      async getCalendarView(start: string, end: string, timeZone: string): Promise<MicrosoftGraph.Event[]> {
        try {
          // GET /me/calendarview?startDateTime=''&endDateTime=''
          // &$select=subject,organizer,start,end
          // &$orderby=start/dateTime
          // &$top=50
          let result =  await this.graphClient
            .api('/me/calendarview')
            .header('Prefer', `outlook.timezone="${timeZone}"`)
            .query({
              startDateTime: start,
              endDateTime: end
            })
            .select('subject,organizer,start,end')
            .orderby('start/dateTime')
            .top(50)
            .get();

          return result.value;
        } catch (error) {
          this.alertsService.addError('Could not get events', JSON.stringify(error, null, 2));
        }
      }
    }
    ```

    <span data-ttu-id="4725c-108">Tenga en cuenta lo que está haciendo este código.</span><span class="sxs-lookup"><span data-stu-id="4725c-108">Consider what this code is doing.</span></span>

    - <span data-ttu-id="4725c-109">Inicializa un cliente de Graph en el constructor del servicio.</span><span class="sxs-lookup"><span data-stu-id="4725c-109">It initializes a Graph client in the constructor for the service.</span></span>
    - <span data-ttu-id="4725c-110">Implementa una `getCalendarView` función que usa el cliente de Graph de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="4725c-110">It implements a `getCalendarView` function that uses the Graph client in the following way:</span></span>
      - <span data-ttu-id="4725c-111">La dirección URL a la que se llamará es `/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="4725c-111">The URL that will be called is `/me/calendarview`.</span></span>
      - <span data-ttu-id="4725c-112">El `header` método incluye el `Prefer: outlook.timezone` encabezado, lo que hace que las horas de inicio y finalización de los eventos devueltos se encuentren en la zona horaria preferida del usuario.</span><span class="sxs-lookup"><span data-stu-id="4725c-112">The `header` method includes the `Prefer: outlook.timezone` header, which causes the start and end times of the returned events to be in the user's preferred time zone.</span></span>
      - <span data-ttu-id="4725c-113">El `query` método agrega los `startDateTime` `endDateTime` parámetros y, que define la ventana de tiempo para la vista de calendario.</span><span class="sxs-lookup"><span data-stu-id="4725c-113">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
      - <span data-ttu-id="4725c-114">El `select` método limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.</span><span class="sxs-lookup"><span data-stu-id="4725c-114">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
      - <span data-ttu-id="4725c-115">El `orderby` método ordena los resultados por hora de inicio.</span><span class="sxs-lookup"><span data-stu-id="4725c-115">The `orderby` method sorts the results by start time.</span></span>

1. <span data-ttu-id="4725c-116">Cree un componente angular para llamar a este nuevo método y mostrar los resultados de la llamada.</span><span class="sxs-lookup"><span data-stu-id="4725c-116">Create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="4725c-117">Ejecute el siguiente comando en su CLI.</span><span class="sxs-lookup"><span data-stu-id="4725c-117">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component calendar
    ```

1. <span data-ttu-id="4725c-118">Una vez que finalice el comando, agregue el componente a la `routes` matriz de **./src/app/app-Routing.Module.ts**.</span><span class="sxs-lookup"><span data-stu-id="4725c-118">Once the command completes, add the component to the `routes` array in **./src/app/app-routing.module.ts**.</span></span>

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. <span data-ttu-id="4725c-119">Abra **./src/App/Calendar/Calendar.Component.ts** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="4725c-119">Open **./src/app/calendar/calendar.component.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';
    import { findOneIana } from 'windows-iana';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';

    import { AuthService } from '../auth.service';
    import { GraphService } from '../graph.service';
    import { AlertsService } from '../alerts.service';

    @Component({
      selector: 'app-calendar',
      templateUrl: './calendar.component.html',
      styleUrls: ['./calendar.component.css']
    })
    export class CalendarComponent implements OnInit {

      public events: MicrosoftGraph.Event[];

      constructor(
        private authService: AuthService,
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        // Convert the user's timezone to IANA format
        const ianaName = findOneIana(this.authService.user.timeZone);
        const timeZone = ianaName!.valueOf() || this.authService.user.timeZone;

        // Get midnight on the start of the current week in the user's timezone,
        // but in UTC. For example, for Pacific Standard Time, the time value would be
        // 07:00:00Z
        var startOfWeek = moment.tz(timeZone).startOf('week').utc();
        var endOfWeek = moment(startOfWeek).add(7, 'day');

        this.graphService.getCalendarView(
          startOfWeek.format(),
          endOfWeek.format(),
          this.authService.user.timeZone)
            .then((events) => {
              this.events = events;
              // Temporary to display raw results
              this.alertsService.addSuccess('Events from Graph', JSON.stringify(events, null, 2));
            });
      }
    }
    ```

<span data-ttu-id="4725c-120">Por ahora, esto solo representa la matriz de eventos en JSON en la página.</span><span class="sxs-lookup"><span data-stu-id="4725c-120">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="4725c-121">Guarde los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4725c-121">Save your changes and restart the app.</span></span> <span data-ttu-id="4725c-122">Inicie sesión y haga clic en el vínculo de **calendario** en la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="4725c-122">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="4725c-123">Si todo funciona, debería ver un volcado JSON de eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="4725c-123">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="4725c-124">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="4725c-124">Display the results</span></span>

<span data-ttu-id="4725c-125">Ahora puede actualizar el `CalendarComponent` componente para mostrar los eventos de forma más fácil de uso.</span><span class="sxs-lookup"><span data-stu-id="4725c-125">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="4725c-126">Quite el código temporal que agrega una alerta de la `ngOnInit` función.</span><span class="sxs-lookup"><span data-stu-id="4725c-126">Remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="4725c-127">La función actualizada debería tener este aspecto.</span><span class="sxs-lookup"><span data-stu-id="4725c-127">Your updated function should look like this.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. <span data-ttu-id="4725c-128">Agregue una función a la `CalendarComponent` clase para dar formato a un `DateTimeTimeZone` objeto en una cadena ISO.</span><span class="sxs-lookup"><span data-stu-id="4725c-128">Add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. <span data-ttu-id="4725c-129">Abra **./src/app/calendar/calendar.component.html** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="4725c-129">Open **./src/app/calendar/calendar.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

<span data-ttu-id="4725c-130">Esto recorre la colección de eventos y agrega una fila de tabla para cada uno.</span><span class="sxs-lookup"><span data-stu-id="4725c-130">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="4725c-131">Guarde los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4725c-131">Save the changes and restart the app.</span></span> <span data-ttu-id="4725c-132">Haga clic en el vínculo del **calendario** y la aplicación ahora debe representar una tabla de eventos.</span><span class="sxs-lookup"><span data-stu-id="4725c-132">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
