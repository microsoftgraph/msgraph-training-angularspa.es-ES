<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b5f9b-101">En este ejercicio, creará un nuevo registro de aplicaciones Web de Azure AD mediante el portal de registro de aplicaciones (ARP).</span><span class="sxs-lookup"><span data-stu-id="b5f9b-101">In this exercise, you will create a new Azure AD web application registration using the Application Registry Portal (ARP).</span></span>

1. <span data-ttu-id="b5f9b-102">Abra un explorador y vaya al [portal de registro de aplicaciones](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b5f9b-102">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="b5f9b-103">Inicie sesión con una **cuenta personal** (también conocido como Microsoft Account) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="b5f9b-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="b5f9b-104">Seleccione **Agregar una aplicación** en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="b5f9b-104">Select **Add an app** at the top of the page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5f9b-105">Si ve más de un botón **Agregar una aplicación** en la página, seleccione el que corresponda a la lista de **aplicaciones convergentes** .</span><span class="sxs-lookup"><span data-stu-id="b5f9b-105">If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="b5f9b-106">En la página **registrar la aplicación** , establezca el tutorial **nombre** de la aplicación en **gráfico angular** y seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="b5f9b-106">On the **Register your application** page, set the **Application Name** to **Angular Graph Tutorial** and select **Create**.</span></span>

    ![Captura de pantalla de la creación de una nueva aplicación en el sitio web del portal de registro de aplicaciones](./images/arp-create-app-01.png)

1. <span data-ttu-id="b5f9b-108">En la página **registro del tutorial de gráficos** de angular, en la sección **propiedades** , copie el identificador de la **aplicación** , ya que lo necesitará más adelante.</span><span class="sxs-lookup"><span data-stu-id="b5f9b-108">On the **Angular Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Captura de pantalla del identificador de la aplicación recién creada](./images/arp-create-app-02.png)

1. <span data-ttu-id="b5f9b-110">Desplácese hacia abajo hasta la sección **plataformas** .</span><span class="sxs-lookup"><span data-stu-id="b5f9b-110">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="b5f9b-111">Seleccione **Agregar plataforma**.</span><span class="sxs-lookup"><span data-stu-id="b5f9b-111">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="b5f9b-112">En el cuadro de diálogo **Agregar plataforma** , seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="b5f9b-112">In the **Add Platform** dialog, select **Web**.</span></span>

        ![Captura de pantalla que crea una plataforma para la aplicación](./images/arp-create-app-03.png)

    1. <span data-ttu-id="b5f9b-114">En el cuadro plataforma **Web** , escriba `http://localhost:4200` para las **direcciones URL**de redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="b5f9b-114">In the **Web** platform box, enter `http://localhost:4200` for the **Redirect URLs**.</span></span>

        ![Captura de pantalla de la plataforma web recién agregada para la aplicación](./images/arp-create-app-04.png)

1. <span data-ttu-id="b5f9b-116">Desplácese hasta la parte inferior de la página y seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="b5f9b-116">Scroll to the bottom of the page and select **Save**.</span></span>