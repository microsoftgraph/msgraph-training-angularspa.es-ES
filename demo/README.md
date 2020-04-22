# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="c0825-101">Cómo ejecutar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="c0825-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c0825-102">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="c0825-102">Prerequisites</span></span>

<span data-ttu-id="c0825-103">Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c0825-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="c0825-104">[Node. js](https://nodejs.org) instalado en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="c0825-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="c0825-105">Si no tiene node. js, visite el vínculo anterior para las opciones de descarga.</span><span class="sxs-lookup"><span data-stu-id="c0825-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="c0825-106">(**Nota:** este tutorial se ha escrito con la versión de nodo 12.16.1.</span><span class="sxs-lookup"><span data-stu-id="c0825-106">(**Note:** This tutorial was written with Node version 12.16.1.</span></span> <span data-ttu-id="c0825-107">Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.</span><span class="sxs-lookup"><span data-stu-id="c0825-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="c0825-108">[Angular](https://cli.angular.io/) de la CLI instalada en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="c0825-108">[Angular CLI](https://cli.angular.io/) installed on your development machine.</span></span>
- <span data-ttu-id="c0825-109">Una cuenta de Microsoft personal con un buzón de correo en Outlook.com o una cuenta profesional o educativa de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c0825-109">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="c0825-110">Si no tiene una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:</span><span class="sxs-lookup"><span data-stu-id="c0825-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="c0825-111">Puede [registrarse para obtener una nueva cuenta Microsoft personal](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="c0825-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="c0825-112">Puede [registrarse para el programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.</span><span class="sxs-lookup"><span data-stu-id="c0825-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="c0825-113">Registro de una aplicación web con el centro de administración de Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c0825-113">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="c0825-114">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c0825-114">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="c0825-115">Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="c0825-115">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="c0825-116">Seleccione **Azure Active Directory** en el panel de navegación de la izquierda y, después, seleccione **registros de aplicaciones** en **administrar**.</span><span class="sxs-lookup"><span data-stu-id="c0825-116">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="c0825-117">Una captura de pantalla de los registros de la aplicación</span><span class="sxs-lookup"><span data-stu-id="c0825-117">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="c0825-118">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="c0825-118">Select **New registration**.</span></span> <span data-ttu-id="c0825-119">En la página **Registrar una aplicación**, establezca los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="c0825-119">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="c0825-120">Establezca **Nombre** como `Angular Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="c0825-120">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="c0825-121">Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="c0825-121">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="c0825-122">En **URI de redirección**, establezca la primera lista desplegable en `Web` y establezca el valor `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="c0825-122">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:4200`.</span></span>

    ![Captura de pantalla de la página registrar una aplicación](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="c0825-124">Elija **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="c0825-124">Choose **Register**.</span></span> <span data-ttu-id="c0825-125">En la página **tutorial de gráfico de angular** , copie el valor del identificador de la **aplicación (cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="c0825-125">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="c0825-127">Seleccione **Autenticación** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="c0825-127">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="c0825-128">Busque la sección **concesión implícita** y habilite **tokens de acceso** y **tokens de identificador**.</span><span class="sxs-lookup"><span data-stu-id="c0825-128">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="c0825-129">Elija **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="c0825-129">Choose **Save**.</span></span>

    ![Captura de pantalla de la sección de concesión implícita](/tutorial/images/aad-implicit-grant.png)

## <a name="configure-the-sample"></a><span data-ttu-id="c0825-131">Configuración del ejemplo</span><span class="sxs-lookup"><span data-stu-id="c0825-131">Configure the sample</span></span>

1. <span data-ttu-id="c0825-132">Cambie el nombre `oauth.ts.example` del archivo `oauth.ts`a.</span><span class="sxs-lookup"><span data-stu-id="c0825-132">Rename the `oauth.ts.example` file to `oauth.ts`.</span></span>
1. <span data-ttu-id="c0825-133">Edite `oauth.ts` el archivo y realice los cambios siguientes.</span><span class="sxs-lookup"><span data-stu-id="c0825-133">Edit the `oauth.ts` file and make the following changes.</span></span>
    1. <span data-ttu-id="c0825-134">Reemplace `YOUR_APP_ID_HERE` por el **identificador de aplicación** que obtuvo desde el portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="c0825-134">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="c0825-135">En la interfaz de línea de comandos (CLI), navegue a este directorio y ejecute el siguiente comando para instalar los requisitos.</span><span class="sxs-lookup"><span data-stu-id="c0825-135">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="c0825-136">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="c0825-136">Run the sample</span></span>

1. <span data-ttu-id="c0825-137">Ejecute el siguiente comando en su CLI para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c0825-137">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    ng serve
    ```

1. <span data-ttu-id="c0825-138">Abra un explorador y vaya a `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="c0825-138">Open a browser and browse to `http://localhost:4200`.</span></span>
