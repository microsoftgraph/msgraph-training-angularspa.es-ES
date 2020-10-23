<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a032c-101">En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="a032c-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="a032c-102">Abra **./src/App/Graph.Service.ts** y agregue la siguiente función a la `GraphService` clase.</span><span class="sxs-lookup"><span data-stu-id="a032c-102">Open **./src/app/graph.service.ts** and add the following function to the `GraphService` class.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="AddEventSnippet":::

## <a name="create-a-new-event-form"></a><span data-ttu-id="a032c-103">Crear un nuevo formulario de eventos</span><span class="sxs-lookup"><span data-stu-id="a032c-103">Create a new event form</span></span>

1. <span data-ttu-id="a032c-104">Cree un componente angular para mostrar un formulario y llamar a esta función nueva.</span><span class="sxs-lookup"><span data-stu-id="a032c-104">Create an Angular component to display a form and call this new function.</span></span> <span data-ttu-id="a032c-105">Ejecute el siguiente comando en su CLI.</span><span class="sxs-lookup"><span data-stu-id="a032c-105">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component new-event
    ```

1. <span data-ttu-id="a032c-106">Una vez que finalice el comando, agregue el componente a la `routes` matriz de **./src/app/app-Routing.Module.ts**.</span><span class="sxs-lookup"><span data-stu-id="a032c-106">Once the command completes, add the component to the `routes` array in **./src/app/app-routing.module.ts**.</span></span>

    ```typescript
    import { NewEventComponent } from './new-event/new-event.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent },
      { path: 'newevent', component: NewEventComponent },
    ];
    ```

1. <span data-ttu-id="a032c-107">Cree un archivo nuevo en el directorio **./src/App/New-Event** denominado **New-Event. ts** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="a032c-107">Create a new file in the **./src/app/new-event** directory named **new-event.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/new-event/new-event.ts" id="NewEventSnippet":::

    <span data-ttu-id="a032c-108">Esta clase actuará como modelo para el nuevo formulario de eventos.</span><span class="sxs-lookup"><span data-stu-id="a032c-108">This class will serve as the model for the new event form.</span></span>

1. <span data-ttu-id="a032c-109">Abra **./src/App/New-Event/New-Event.Component.ts** y reemplace su contenido por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="a032c-109">Open **./src/app/new-event/new-event.component.ts** and replace its contents with the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/new-event/new-event.component.ts" id="NewEventComponentSnippet":::

1. <span data-ttu-id="a032c-110">Abra **./src/app/new-event/new-event.component.html** y reemplace su contenido por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="a032c-110">Open **./src/app/new-event/new-event.component.html** and replace its contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/new-event/new-event.component.html" id="NewEventFormSnippet":::

1. <span data-ttu-id="a032c-111">Guarde los cambios y actualice la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a032c-111">Save the changes and refresh the app.</span></span> <span data-ttu-id="a032c-112">Seleccione el botón **nuevo evento** en la página calendario y, a continuación, use el formulario para crear un evento en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="a032c-112">Select the **New event** button on the calendar page, then use the form to create an event on the user's calendar.</span></span>

    ![Captura de pantalla del nuevo formulario de eventos](images/create-event.png)
