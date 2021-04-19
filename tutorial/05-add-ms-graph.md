<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, incorporará Microsoft Graph a la aplicación. Para esta aplicación, usará la biblioteca [de microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obtener eventos del calendario desde Outlook

1. Agregue un nuevo servicio para contener todas las llamadas de Graph. Ejecute el siguiente comando en la CLI.

    ```Shell
    ng generate service graph
    ```

    Al igual que con el servicio de autenticación que creó anteriormente, la creación de un servicio para esto le permite insertarlo en los componentes que necesitan acceso a Microsoft Graph.

1. Una vez completado el comando, abra **./src/app/graph.service.ts** y reemplace su contenido por lo siguiente.

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
            const token = await this.authService.getAccessToken()
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

      async getCalendarView(start: string, end: string, timeZone: string): Promise<MicrosoftGraph.Event[] | undefined> {
        try {
          // GET /me/calendarview?startDateTime=''&endDateTime=''
          // &$select=subject,organizer,start,end
          // &$orderby=start/dateTime
          // &$top=50
          const result =  await this.graphClient
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
        return undefined;
      }
    }
    ```

    Tenga en cuenta lo que está haciendo este código.

    - Inicializa un cliente de Graph en el constructor del servicio.
    - Implementa una función `getCalendarView` que usa el cliente de Graph de la siguiente manera:
      - La dirección URL a la que se llamará es `/me/calendarview`.
      - El método incluye el encabezado, lo que hace que las horas de inicio y finalización de los eventos devueltos se incluyan en la zona horaria preferida `header` `Prefer: outlook.timezone` del usuario.
      - El `query` método agrega los parámetros `startDateTime` `endDateTime` y, definiendo la ventana de tiempo para la vista de calendario.
      - El `select` método limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.
      - El `orderby` método ordena los resultados por hora de inicio.

1. Cree un componente angular para llamar a este nuevo método y mostrar los resultados de la llamada. Ejecute el siguiente comando en la CLI.

    ```Shell
    ng generate component calendar
    ```

1. Una vez completado el comando, agregue el componente a la `routes` matriz **en ./src/app/app-routing.module.ts**.

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent },
    ];
    ```

1. Abra **./tsconfig.jsy** agregue la siguiente propiedad al `compilerOptions` objeto.

    ```json
    "resolveJsonModule": true
    ```

1. Abra **./src/app/calendar/calendar.component.ts** y reemplace su contenido por lo siguiente.

    ```typescript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';
    import { findIana } from 'windows-iana';
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

      public events?: MicrosoftGraph.Event[];

      constructor(
        private authService: AuthService,
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        // Convert the user's timezone to IANA format
        const ianaName = findIana(this.authService.user?.timeZone ?? 'UTC');
        const timeZone = ianaName![0].valueOf() || this.authService.user?.timeZone || 'UTC';

        // Get midnight on the start of the current week in the user's timezone,
        // but in UTC. For example, for Pacific Standard Time, the time value would be
        // 07:00:00Z
        var startOfWeek = moment.tz(timeZone).startOf('week').utc();
        var endOfWeek = moment(startOfWeek).add(7, 'day');

        this.graphService.getCalendarView(
          startOfWeek.format(),
          endOfWeek.format(),
          this.authService.user?.timeZone ?? 'UTC')
            .then((events) => {
              this.events = events;
              // Temporary to display raw results
              this.alertsService.addSuccess('Events from Graph', JSON.stringify(events, null, 2));
            });
      }
    }
    ```

Por ahora, esto simplemente representa la matriz de eventos en JSON en la página. Guarde los cambios y reinicie la aplicación. Inicie sesión y haga clic en **el vínculo Calendario** de la barra de navegación. Si funciona todo, debería ver un volcado JSON de eventos en el calendario del usuario.

## <a name="display-the-results"></a>Mostrar los resultados

Ahora puede actualizar el componente para mostrar los eventos de una manera más `CalendarComponent` fácil de usar.

1. Quite el código temporal que agrega una alerta de la `ngOnInit` función. La función actualizada debe tener este aspecto.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. Agregue una función a la `CalendarComponent` clase para dar formato a un objeto en una cadena `DateTimeTimeZone` ISO.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. Abra **./src/app/calendar/calendar.component.html** y reemplace su contenido por lo siguiente.

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

Esto recorre la colección de eventos y agrega una fila de tabla para cada uno. Guarda los cambios y reinicia la aplicación. Haz clic en **el vínculo** Calendario y la aplicación ahora debe representar una tabla de eventos.

![Una captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
