<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="2fc5a-101">En este ejercicio, creará un nuevo registro de aplicaciones web de Azure AD mediante el Centro de administración de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2fc5a-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="2fc5a-102">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2fc5a-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="2fc5a-103">Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="2fc5a-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="2fc5a-104">Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="2fc5a-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="2fc5a-105">Captura de pantalla de los registros de la aplicación</span><span class="sxs-lookup"><span data-stu-id="2fc5a-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="2fc5a-106">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="2fc5a-106">Select **New registration**.</span></span> <span data-ttu-id="2fc5a-107">En la página **Registrar una aplicación**, establezca los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="2fc5a-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="2fc5a-108">Establezca **Nombre** como `Angular Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="2fc5a-108">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="2fc5a-109">Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="2fc5a-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="2fc5a-110">En **URI de redirección**, establezca la primera lista desplegable en `Single-page application (SPA)` y establezca el valor `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="2fc5a-110">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `http://localhost:4200`.</span></span>

    ![Captura de pantalla de la página Registrar una aplicación](./images/aad-register-an-app.png)

1. <span data-ttu-id="2fc5a-112">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="2fc5a-112">Select **Register**.</span></span> <span data-ttu-id="2fc5a-113">En la **página Tutorial de Angular Graph,** copie el valor del identificador de aplicación **(cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="2fc5a-113">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Una captura de pantalla del Id. de aplicación del nuevo registro de la aplicación](./images/aad-application-id.png)
