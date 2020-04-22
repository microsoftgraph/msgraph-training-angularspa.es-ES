<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="60a20-101">En este ejercicio, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="60a20-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="60a20-102">Para esta aplicación, usará la biblioteca de [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="60a20-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="60a20-103">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="60a20-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="60a20-104">Cree un nuevo archivo en el `./src/app` directorio denominado `event.ts` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="60a20-104">Create a new file in the `./src/app` directory called `event.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/event.ts" id="eventClasses":::

1. <span data-ttu-id="60a20-105">Agregue un nuevo servicio para que contenga todas las llamadas de gráfico.</span><span class="sxs-lookup"><span data-stu-id="60a20-105">Add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="60a20-106">Ejecute el siguiente comando en su CLI.</span><span class="sxs-lookup"><span data-stu-id="60a20-106">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service graph
    ```

    <span data-ttu-id="60a20-107">Al igual que con el servicio de autenticación que creó anteriormente, la creación de un servicio le permite inyectarlo en cualquier componente que necesite acceso a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="60a20-107">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span>

1. <span data-ttu-id="60a20-108">Una vez que haya finalizado el comando `./src/app/graph.service.ts` , abra el archivo y reemplace el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="60a20-108">Once the command completes, open the `./src/app/graph.service.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="graphServiceSnippet":::

    <span data-ttu-id="60a20-109">Tenga en cuenta lo que está haciendo este código.</span><span class="sxs-lookup"><span data-stu-id="60a20-109">Consider what this code is doing.</span></span>

    - <span data-ttu-id="60a20-110">Inicializa un cliente de Graph en el constructor del servicio.</span><span class="sxs-lookup"><span data-stu-id="60a20-110">It initializes a Graph client in the constructor for the service.</span></span>
    - <span data-ttu-id="60a20-111">Implementa una `getEvents` función que usa el cliente de Graph de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="60a20-111">It implements a `getEvents` function that uses the Graph client in the following way:</span></span>
      - <span data-ttu-id="60a20-112">La dirección URL a la que se `/me/events`llamará es.</span><span class="sxs-lookup"><span data-stu-id="60a20-112">The URL that will be called is `/me/events`.</span></span>
      - <span data-ttu-id="60a20-113">El `select` método limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.</span><span class="sxs-lookup"><span data-stu-id="60a20-113">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
      - <span data-ttu-id="60a20-114">El `orderby` método ordena los resultados por la fecha y hora en que se crearon, con el elemento más reciente en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="60a20-114">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="60a20-115">Cree un componente angular para llamar a este nuevo método y mostrar los resultados de la llamada.</span><span class="sxs-lookup"><span data-stu-id="60a20-115">Create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="60a20-116">Ejecute el siguiente comando en su CLI.</span><span class="sxs-lookup"><span data-stu-id="60a20-116">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component calendar
    ```

1. <span data-ttu-id="60a20-117">Una vez que finalice el comando, agregue el componente a `routes` la matriz `./src/app/app-routing.module.ts`de.</span><span class="sxs-lookup"><span data-stu-id="60a20-117">Once the command completes, add the component to the `routes` array in `./src/app/app-routing.module.ts`.</span></span>

    ```TypeScript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. <span data-ttu-id="60a20-118">Abra el `./src/app/calendar/calendar.component.ts` archivo y reemplace el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="60a20-118">Open the `./src/app/calendar/calendar.component.ts` file and replace its contents with the following.</span></span>

    ```TypeScript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';

    import { GraphService } from '../graph.service';
    import { Event, DateTimeTimeZone } from '../event';
    import { AlertsService } from '../alerts.service';

    @Component({
      selector: 'app-calendar',
      templateUrl: './calendar.component.html',
      styleUrls: ['./calendar.component.css']
    })
    export class CalendarComponent implements OnInit {

      public events: Event[];

      constructor(
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        this.graphService.getEvents()
          .then((events) => {
            this.events = events;
            // Temporary to display raw results
            this.alertsService.add('Events from Graph', JSON.stringify(events, null, 2));
          });
      }
    }
    ```

<span data-ttu-id="60a20-119">Por ahora, esto solo representa la matriz de eventos en JSON en la página.</span><span class="sxs-lookup"><span data-stu-id="60a20-119">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="60a20-120">Guarde los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="60a20-120">Save your changes and restart the app.</span></span> <span data-ttu-id="60a20-121">Inicie sesión y haga clic en el vínculo de **calendario** en la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="60a20-121">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="60a20-122">Si todo funciona, debería ver un volcado JSON de eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="60a20-122">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="60a20-123">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="60a20-123">Display the results</span></span>

<span data-ttu-id="60a20-124">Ahora puede actualizar el `CalendarComponent` componente para mostrar los eventos de forma más fácil de uso.</span><span class="sxs-lookup"><span data-stu-id="60a20-124">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="60a20-125">Quite el código temporal que agrega una alerta de la `ngOnInit` función.</span><span class="sxs-lookup"><span data-stu-id="60a20-125">Remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="60a20-126">La función actualizada debería tener este aspecto.</span><span class="sxs-lookup"><span data-stu-id="60a20-126">Your updated function should look like this.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. <span data-ttu-id="60a20-127">Agregue una función a la `CalendarComponent` clase para dar formato `DateTimeTimeZone` a un objeto en una cadena ISO.</span><span class="sxs-lookup"><span data-stu-id="60a20-127">Add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. <span data-ttu-id="60a20-128">Abra el `./src/app/calendar/calendar.component.html` archivo y reemplace el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="60a20-128">Open the `./src/app/calendar/calendar.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

<span data-ttu-id="60a20-129">Esto recorre la colección de eventos y agrega una fila de tabla para cada uno.</span><span class="sxs-lookup"><span data-stu-id="60a20-129">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="60a20-130">Guarde los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="60a20-130">Save the changes and restart the app.</span></span> <span data-ttu-id="60a20-131">Haga clic en el vínculo del **calendario** y la aplicación ahora debe representar una tabla de eventos.</span><span class="sxs-lookup"><span data-stu-id="60a20-131">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
