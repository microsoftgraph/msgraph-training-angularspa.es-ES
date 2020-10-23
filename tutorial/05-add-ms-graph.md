<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, incorporará Microsoft Graph a la aplicación. Para esta aplicación, usará la biblioteca de [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obtener eventos de calendario de Outlook

1. Agregue un nuevo servicio para que contenga todas las llamadas de gráfico. Ejecute el siguiente comando en su CLI.

    ```Shell
    ng generate service graph
    ```

    Al igual que con el servicio de autenticación que creó anteriormente, la creación de un servicio le permite inyectarlo en cualquier componente que necesite acceso a Microsoft Graph.

1. Una vez que finalice el comando, Abra **./src/App/Graph.Service.ts** y reemplace su contenido por lo siguiente.

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

    Tenga en cuenta lo que está haciendo este código.

    - Inicializa un cliente de Graph en el constructor del servicio.
    - Implementa una `getCalendarView` función que usa el cliente de Graph de la siguiente manera:
      - La dirección URL a la que se llamará es `/me/calendarview` .
      - El `header` método incluye el `Prefer: outlook.timezone` encabezado, lo que hace que las horas de inicio y finalización de los eventos devueltos se encuentren en la zona horaria preferida del usuario.
      - El `query` método agrega los `startDateTime` `endDateTime` parámetros y, que define la ventana de tiempo para la vista de calendario.
      - El `select` método limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.
      - El `orderby` método ordena los resultados por hora de inicio.

1. Cree un componente angular para llamar a este nuevo método y mostrar los resultados de la llamada. Ejecute el siguiente comando en su CLI.

    ```Shell
    ng generate component calendar
    ```

1. Una vez que finalice el comando, agregue el componente a la `routes` matriz de **./src/app/app-Routing.Module.ts**.

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. Abra **./src/App/Calendar/Calendar.Component.ts** y reemplace su contenido por lo siguiente.

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

Por ahora, esto solo representa la matriz de eventos en JSON en la página. Guarde los cambios y reinicie la aplicación. Inicie sesión y haga clic en el vínculo de **calendario** en la barra de navegación. Si todo funciona, debería ver un volcado JSON de eventos en el calendario del usuario.

## <a name="display-the-results"></a>Mostrar los resultados

Ahora puede actualizar el `CalendarComponent` componente para mostrar los eventos de forma más fácil de uso.

1. Quite el código temporal que agrega una alerta de la `ngOnInit` función. La función actualizada debería tener este aspecto.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. Agregue una función a la `CalendarComponent` clase para dar formato a un `DateTimeTimeZone` objeto en una cadena ISO.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. Abra **./src/app/calendar/calendar.component.html** y reemplace su contenido por lo siguiente.

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

Esto recorre la colección de eventos y agrega una fila de tabla para cada uno. Guarde los cambios y reinicie la aplicación. Haga clic en el vínculo del **calendario** y la aplicación ahora debe representar una tabla de eventos.

![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
