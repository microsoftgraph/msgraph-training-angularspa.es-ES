# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="0275c-101">Cómo ejecutar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="0275c-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0275c-102">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="0275c-102">Prerequisites</span></span>

<span data-ttu-id="0275c-103">Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0275c-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="0275c-104">[Node. js](https://nodejs.org) instalado en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="0275c-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="0275c-105">Si no tiene node. js, visite el vínculo anterior para las opciones de descarga.</span><span class="sxs-lookup"><span data-stu-id="0275c-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="0275c-106">(**Nota:** este tutorial se ha escrito con la versión de nodo 10.7.0.</span><span class="sxs-lookup"><span data-stu-id="0275c-106">(**Note:** This tutorial was written with Node version 10.7.0.</span></span> <span data-ttu-id="0275c-107">Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.</span><span class="sxs-lookup"><span data-stu-id="0275c-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="0275c-108">Angular de la [CLI](https://cli.angular.io/) instalada en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="0275c-108">[Angular CLI](https://cli.angular.io/) installed on your development machine.</span></span>
- <span data-ttu-id="0275c-109">Una cuenta de Microsoft personal con un buzón de correo en Outlook.com o una cuenta profesional o educativa de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0275c-109">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="0275c-110">Si no tiene una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:</span><span class="sxs-lookup"><span data-stu-id="0275c-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="0275c-111">Puede [registrarse para obtener una nueva cuenta Microsoft personal](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="0275c-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="0275c-112">Puede [registrarse para el programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.</span><span class="sxs-lookup"><span data-stu-id="0275c-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-application-registration-portal"></a><span data-ttu-id="0275c-113">Registro de una aplicación web con el portal de registro de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="0275c-113">Register a web application with the Application Registration Portal</span></span>

1. <span data-ttu-id="0275c-114">Abra un explorador y vaya al [portal de registro de aplicaciones](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="0275c-114">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="0275c-115">Inicie sesión con una **cuenta personal** (también conocido como Microsoft Account) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="0275c-115">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="0275c-116">Seleccione **Agregar una aplicación** en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="0275c-116">Select **Add an app** at the top of the page.</span></span>

    > <span data-ttu-id="0275c-117">**Nota:** Si ve más de un botón **Agregar una aplicación** en la página, seleccione el que corresponda a la lista de **aplicaciones convergentes** .</span><span class="sxs-lookup"><span data-stu-id="0275c-117">**Note:** If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="0275c-118">En la página **registrar la aplicación** , establezca el tutorial **nombre** de la aplicación en **gráfico angular** y seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="0275c-118">On the **Register your application** page, set the **Application Name** to **Angular Graph Tutorial** and select **Create**.</span></span>

    ![Captura de pantalla de la creación de una nueva aplicación en el sitio web del portal de registro de aplicaciones](/tutorial/images/arp-create-app-01.png)

1. <span data-ttu-id="0275c-120">En la página **registro del tutorial de gráficos** de angular, en la sección **propiedades** , copie el identificador de la **aplicación** , ya que lo necesitará más adelante.</span><span class="sxs-lookup"><span data-stu-id="0275c-120">On the **Angular Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Captura de pantalla del identificador de la aplicación recién creada](/tutorial/images/arp-create-app-02.png)

1. <span data-ttu-id="0275c-122">Desplácese hacia abajo hasta la sección **plataformas** .</span><span class="sxs-lookup"><span data-stu-id="0275c-122">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="0275c-123">Seleccione **Agregar plataforma**.</span><span class="sxs-lookup"><span data-stu-id="0275c-123">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="0275c-124">En el cuadro de diálogo **Agregar plataforma** , seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="0275c-124">In the **Add Platform** dialog, select **Web**.</span></span>

        ![Captura de pantalla que crea una plataforma para la aplicación](/tutorial/images/arp-create-app-03.png)

    1. <span data-ttu-id="0275c-126">En el cuadro plataforma **Web** , escriba la dirección `http://localhost:4200` URL de las **direcciones URL**de redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="0275c-126">In the **Web** platform box, enter the URL `http://localhost:4200` for the **Redirect URLs**.</span></span>

        ![Captura de pantalla de la plataforma web recién agregada para la aplicación](/tutorial/images/arp-create-app-04.png)

1. <span data-ttu-id="0275c-128">Desplácese hasta la parte inferior de la página y seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="0275c-128">Scroll to the bottom of the page and select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="0275c-129">Configuración del ejemplo</span><span class="sxs-lookup"><span data-stu-id="0275c-129">Configure the sample</span></span>

1. <span data-ttu-id="0275c-130">Cambie el nombre `oauth.ts.example` del archivo `oauth.ts`a.</span><span class="sxs-lookup"><span data-stu-id="0275c-130">Rename the `oauth.ts.example` file to `oauth.ts`.</span></span>
1. <span data-ttu-id="0275c-131">Edite `oauth.ts` el archivo y realice los cambios siguientes.</span><span class="sxs-lookup"><span data-stu-id="0275c-131">Edit the `oauth.ts` file and make the following changes.</span></span>
    1. <span data-ttu-id="0275c-132">Reemplace `YOUR_APP_ID_HERE` por el **identificador de aplicación** que obtuvo desde el portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="0275c-132">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="0275c-133">En la interfaz de línea de comandos (CLI), navegue a este directorio y ejecute el siguiente comando para instalar los requisitos.</span><span class="sxs-lookup"><span data-stu-id="0275c-133">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="0275c-134">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0275c-134">Run the sample</span></span>

1. <span data-ttu-id="0275c-135">Ejecute el siguiente comando en su CLI para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0275c-135">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    ng serve
    ```

1. <span data-ttu-id="0275c-136">Abra un explorador y vaya a `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="0275c-136">Open a browser and browse to `http://localhost:4200`.</span></span>