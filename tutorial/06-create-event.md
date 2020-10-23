<!-- markdownlint-disable MD002 MD041 -->

En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.

1. Abra **./src/App/Graph.Service.ts** y agregue la siguiente función a la `GraphService` clase.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="AddEventSnippet":::

## <a name="create-a-new-event-form"></a>Crear un nuevo formulario de eventos

1. Cree un componente angular para mostrar un formulario y llamar a esta función nueva. Ejecute el siguiente comando en su CLI.

    ```Shell
    ng generate component new-event
    ```

1. Una vez que finalice el comando, agregue el componente a la `routes` matriz de **./src/app/app-Routing.Module.ts**.

    ```typescript
    import { NewEventComponent } from './new-event/new-event.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent },
      { path: 'newevent', component: NewEventComponent },
    ];
    ```

1. Cree un archivo nuevo en el directorio **./src/App/New-Event** denominado **New-Event. ts** y agregue el siguiente código.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/new-event/new-event.ts" id="NewEventSnippet":::

    Esta clase actuará como modelo para el nuevo formulario de eventos.

1. Abra **./src/App/New-Event/New-Event.Component.ts** y reemplace su contenido por el código siguiente.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/new-event/new-event.component.ts" id="NewEventComponentSnippet":::

1. Abra **./src/app/new-event/new-event.component.html** y reemplace su contenido por el código siguiente.

    :::code language="html" source="../demo/graph-tutorial/src/app/new-event/new-event.component.html" id="NewEventFormSnippet":::

1. Guarde los cambios y actualice la aplicación. Seleccione el botón **nuevo evento** en la página calendario y, a continuación, use el formulario para crear un evento en el calendario del usuario.

    ![Captura de pantalla del nuevo formulario de eventos](images/create-event.png)
