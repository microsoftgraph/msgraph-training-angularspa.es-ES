<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, incorporará Microsoft Graph a la aplicación. Para esta aplicación, usará la biblioteca de [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obtener eventos de calendario de Outlook

1. Cree un nuevo archivo en el `./src/app` directorio denominado `event.ts` y agregue el siguiente código.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/event.ts" id="eventClasses":::

1. Agregue un nuevo servicio para que contenga todas las llamadas de gráfico. Ejecute el siguiente comando en su CLI.

    ```Shell
    ng generate service graph
    ```

    Al igual que con el servicio de autenticación que creó anteriormente, la creación de un servicio le permite inyectarlo en cualquier componente que necesite acceso a Microsoft Graph.

1. Una vez que haya finalizado el comando `./src/app/graph.service.ts` , abra el archivo y reemplace el contenido por lo siguiente.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="graphServiceSnippet":::

    Tenga en cuenta lo que está haciendo este código.

    - Inicializa un cliente de Graph en el constructor del servicio.
    - Implementa una `getEvents` función que usa el cliente de Graph de la siguiente manera:
      - La dirección URL a la que se `/me/events`llamará es.
      - El `select` método limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.
      - El `orderby` método ordena los resultados por la fecha y hora en que se crearon, con el elemento más reciente en primer lugar.

1. Cree un componente angular para llamar a este nuevo método y mostrar los resultados de la llamada. Ejecute el siguiente comando en su CLI.

    ```Shell
    ng generate component calendar
    ```

1. Una vez que finalice el comando, agregue el componente a `routes` la matriz `./src/app/app-routing.module.ts`de.

    ```TypeScript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. Abra el `./src/app/calendar/calendar.component.ts` archivo y reemplace el contenido por lo siguiente.

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

Por ahora, esto solo representa la matriz de eventos en JSON en la página. Guarde los cambios y reinicie la aplicación. Inicie sesión y haga clic en el vínculo de **calendario** en la barra de navegación. Si todo funciona, debería ver un volcado JSON de eventos en el calendario del usuario.

## <a name="display-the-results"></a>Mostrar los resultados

Ahora puede actualizar el `CalendarComponent` componente para mostrar los eventos de forma más fácil de uso.

1. Quite el código temporal que agrega una alerta de la `ngOnInit` función. La función actualizada debería tener este aspecto.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. Agregue una función a la `CalendarComponent` clase para dar formato `DateTimeTimeZone` a un objeto en una cadena ISO.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. Abra el `./src/app/calendar/calendar.component.html` archivo y reemplace el contenido por lo siguiente.

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

Esto recorre la colección de eventos y agrega una fila de tabla para cada uno. Guarde los cambios y reinicie la aplicación. Haga clic en el vínculo del **calendario** y la aplicación ahora debe representar una tabla de eventos.

![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
