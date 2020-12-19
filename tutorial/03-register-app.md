<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4930f-101">В этом упражнении вы создайте регистрацию нового веб-приложения Azure AD с помощью Центра администрирования Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4930f-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="4930f-102">Откройте браузер и перейдите к [Центру администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4930f-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="4930f-103">Войдите с помощью **личной учетной записи** (т.е. учетной записи Microsoft) или **рабочей (учебной) учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="4930f-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="4930f-104">Выберите **Azure Active Directory** на панели навигации слева, затем выберите **Регистрация приложений** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="4930f-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="4930f-105">Снимок экрана с регистрацией приложений</span><span class="sxs-lookup"><span data-stu-id="4930f-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="4930f-106">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="4930f-106">Select **New registration**.</span></span> <span data-ttu-id="4930f-107">На странице **"Регистрация приложения"** задайте значения следующим образом, где находится URL-адрес переадстройки ngrok, скопированный в `YOUR_NGROK_URL` предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="4930f-107">On the **Register an application** page, set the values as follows, where `YOUR_NGROK_URL` is the ngrok forwarding URL you copied in the previous section.</span></span>

    - <span data-ttu-id="4930f-108">Введите **имя** `Teams Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="4930f-108">Set **Name** to `Teams Graph Tutorial`.</span></span>
    - <span data-ttu-id="4930f-109">Введите **поддерживаемые типы учетных записей** для **учетных записей в любом каталоге организаций и личных учетных записей Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="4930f-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="4930f-110">В разделе **URI адрес перенаправления** введите значение в первом раскрывающемся списке `Web` и задайте значение `YOUR_NGROK_URL/authcomplete`.</span><span class="sxs-lookup"><span data-stu-id="4930f-110">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `YOUR_NGROK_URL/authcomplete`.</span></span>

    ![Снимок экрана: страница "Регистрация приложения"](./images/aad-register-an-app.png)

1. <span data-ttu-id="4930f-112">Нажмите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="4930f-112">Select **Register**.</span></span> <span data-ttu-id="4930f-113">На странице **"Руководство по Teams Graph"** скопируйте значение ИД приложения **(клиента)** и сохраните его, оно потребуется вам на следующем этапе.</span><span class="sxs-lookup"><span data-stu-id="4930f-113">On the **Teams Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана: ИД нового приложения для регистрации](./images/aad-application-id.png)

1. <span data-ttu-id="4930f-115">Выберите **Проверка подлинности** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="4930f-115">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="4930f-116">Найдите **раздел неявного предоставления** и в включить **маркеры доступа** и **маркеры ID.**</span><span class="sxs-lookup"><span data-stu-id="4930f-116">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="4930f-117">Нажмите **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="4930f-117">Select **Save**.</span></span>

1. <span data-ttu-id="4930f-118">Выберите **Сертификаты и секреты** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="4930f-118">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="4930f-119">Нажмите кнопку **Новый секрет клиента**.</span><span class="sxs-lookup"><span data-stu-id="4930f-119">Select the **New client secret** button.</span></span> <span data-ttu-id="4930f-120">Введите значение  в описании, выберите один из параметров "Срок действия **истекает"** и выберите **"Добавить".**</span><span class="sxs-lookup"><span data-stu-id="4930f-120">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="4930f-121">Скопируйте значение секрета клиента, а затем покиньте эту страницу.</span><span class="sxs-lookup"><span data-stu-id="4930f-121">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="4930f-122">Оно вам понадобится на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="4930f-122">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="4930f-123">Это секрет клиента, он никогда не отображается еще раз, поэтому убедитесь, что вы скопировали его.</span><span class="sxs-lookup"><span data-stu-id="4930f-123">This client secret is never shown again, so make sure you copy it now.</span></span>

1. <span data-ttu-id="4930f-124">Выберите **разрешения API в** области **"Управление",** а затем **выберите "Добавить разрешение".**</span><span class="sxs-lookup"><span data-stu-id="4930f-124">Select **API permissions** under **Manage**, then select **Add a permission**.</span></span>

1. <span data-ttu-id="4930f-125">Выберите **Microsoft Graph,** а затем **делегирование разрешений.**</span><span class="sxs-lookup"><span data-stu-id="4930f-125">Select **Microsoft Graph**, then **Delegated permissions**.</span></span>

1. <span data-ttu-id="4930f-126">Выберите следующие разрешения, а затем выберите **"Добавить разрешения".**</span><span class="sxs-lookup"><span data-stu-id="4930f-126">Select the following permissions, then select **Add permissions**.</span></span>

    - <span data-ttu-id="4930f-127">**Calendars.ReadWrite** — позволяет приложению читать и записывать данные в календарь пользователя.</span><span class="sxs-lookup"><span data-stu-id="4930f-127">**Calendars.ReadWrite** - this will allow the app to read and write to the user's calendar.</span></span>
    - <span data-ttu-id="4930f-128">**MailboxSettings.Read** — позволяет приложению получать часовой пояс, формат даты и формат времени пользователя из параметров почтового ящика.</span><span class="sxs-lookup"><span data-stu-id="4930f-128">**MailboxSettings.Read** - this will allow the app to get the user's time zone, date format, and time format from their mailbox settings.</span></span>

    ![Снимок экрана с настроенными разрешениями](images/aad-configured-permissions.png)

## <a name="configure-teams-single-sign-on"></a><span data-ttu-id="4930f-130">Настройка единого входов в Teams</span><span class="sxs-lookup"><span data-stu-id="4930f-130">Configure Teams single sign-on</span></span>

<span data-ttu-id="4930f-131">В этом разделе вы обновим регистрацию приложения, чтобы поддерживать единый вход [в Teams.](/microsoftteams/platform/tabs/how-to/authentication/auth-aad-sso)</span><span class="sxs-lookup"><span data-stu-id="4930f-131">In this section you'll update the app registration to support [single sign-on in Teams](/microsoftteams/platform/tabs/how-to/authentication/auth-aad-sso).</span></span>

1. <span data-ttu-id="4930f-132">Выберите **"Показать API"**.</span><span class="sxs-lookup"><span data-stu-id="4930f-132">Select **Expose an API**.</span></span> <span data-ttu-id="4930f-133">Выберите **ссылку "Установить"** рядом с **URI ИД приложения.**</span><span class="sxs-lookup"><span data-stu-id="4930f-133">Select the **Set** link next to **Application ID URI**.</span></span> <span data-ttu-id="4930f-134">Вставьте доменное имя URL-адреса переадружения ngrok (с косой чертой "/" в конце) между двойной косой чертой и GUID.</span><span class="sxs-lookup"><span data-stu-id="4930f-134">Insert your ngrok forwarding URL domain name (with a forward slash "/" appended to the end) between the double forward slashes and the GUID.</span></span> <span data-ttu-id="4930f-135">Весь ИД должен выглядеть примерно так: `api://50153897dd4d.ngrok.io/ae7d8088-3422-4c8c-a351-6ded0f21d615` .</span><span class="sxs-lookup"><span data-stu-id="4930f-135">The entire ID should look similar to: `api://50153897dd4d.ngrok.io/ae7d8088-3422-4c8c-a351-6ded0f21d615`.</span></span>

1. <span data-ttu-id="4930f-136">В разделе **"Области", определяемом этим API,** выберите **"Добавить область".**</span><span class="sxs-lookup"><span data-stu-id="4930f-136">In the **Scopes defined by this API** section, select **Add a scope**.</span></span> <span data-ttu-id="4930f-137">Заполните поля следующим образом и выберите **"Добавить область".**</span><span class="sxs-lookup"><span data-stu-id="4930f-137">Fill in the fields as follows and select **Add scope**.</span></span>

    - <span data-ttu-id="4930f-138">**Имя области:**`access_as_user`</span><span class="sxs-lookup"><span data-stu-id="4930f-138">**Scope name:** `access_as_user`</span></span>
    - <span data-ttu-id="4930f-139">**Кто может дать согласие?: администраторы и пользователи**</span><span class="sxs-lookup"><span data-stu-id="4930f-139">**Who can consent?: Admins and users**</span></span>
    - <span data-ttu-id="4930f-140">**Отображаемая имя согласия администратора:**`Access the app as the user`</span><span class="sxs-lookup"><span data-stu-id="4930f-140">**Admin consent display name:** `Access the app as the user`</span></span>
    - <span data-ttu-id="4930f-141">**Описание согласия администратора:**`Allows Teams to call the app's web APIs as the current user.`</span><span class="sxs-lookup"><span data-stu-id="4930f-141">**Admin consent description:** `Allows Teams to call the app's web APIs as the current user.`</span></span>
    - <span data-ttu-id="4930f-142">**Отображаемая имя согласия пользователя:**`Access the app as you`</span><span class="sxs-lookup"><span data-stu-id="4930f-142">**User consent display name:** `Access the app as you`</span></span>
    - <span data-ttu-id="4930f-143">**Описание согласия пользователя:**`Allows Teams to call the app's web APIs as you.`</span><span class="sxs-lookup"><span data-stu-id="4930f-143">**User consent description:** `Allows Teams to call the app's web APIs as you.`</span></span>
    - <span data-ttu-id="4930f-144">**Состояние: включено**</span><span class="sxs-lookup"><span data-stu-id="4930f-144">**State: Enabled**</span></span>

    ![Снимок экрана с формой "Добавление области"](images/aad-add-scope.png)

1. <span data-ttu-id="4930f-146">В разделе **"Авторизованные клиентские приложения"** выберите **"Добавить клиентские приложения".**</span><span class="sxs-lookup"><span data-stu-id="4930f-146">In the **Authorized client applications** section, select **Add a client application**.</span></span> <span data-ttu-id="4930f-147">Введите ИД клиента из следующего списка, введите область в разрешенных разрешениях и выберите **"Добавить приложение".**</span><span class="sxs-lookup"><span data-stu-id="4930f-147">Enter a client ID from the following list, enable the scope under **Authorized scopes**, and select **Add application**.</span></span> <span data-ttu-id="4930f-148">Повторите этот процесс для каждого из клиентских ИД в списке.</span><span class="sxs-lookup"><span data-stu-id="4930f-148">Repeat this process for each of the client IDs in the list.</span></span>

    - <span data-ttu-id="4930f-149">`1fec8e78-bce4-4aaf-ab1b-5451cc387264` (Мобильное или настольное приложение Teams)</span><span class="sxs-lookup"><span data-stu-id="4930f-149">`1fec8e78-bce4-4aaf-ab1b-5451cc387264` (Teams mobile/desktop application)</span></span>
    - <span data-ttu-id="4930f-150">`5e3ce6c0-2b1f-4285-8d4b-75ee78787346` (Веб-приложение Teams)</span><span class="sxs-lookup"><span data-stu-id="4930f-150">`5e3ce6c0-2b1f-4285-8d4b-75ee78787346` (Teams web application)</span></span>
