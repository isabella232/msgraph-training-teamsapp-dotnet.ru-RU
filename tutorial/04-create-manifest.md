<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="22d1c-101">В манифесте приложения описывается, как приложение интегрируется с Microsoft Teams и требуется для установки приложений.</span><span class="sxs-lookup"><span data-stu-id="22d1c-101">The app manifest describes how the app integrates with Microsoft Teams and is required to install apps.</span></span> <span data-ttu-id="22d1c-102">В этом разделе вы будете использовать App Studio в клиенте Microsoft Teams для создания манифеста.</span><span class="sxs-lookup"><span data-stu-id="22d1c-102">In this section you'll use App Studio in the Microsoft Teams client to generate a manifest.</span></span>

1. <span data-ttu-id="22d1c-103">Если вы еще не установили App Studio в Teams, [установите его сейчас.](/microsoftteams/platform/concepts/build-and-test/app-studio-overview)</span><span class="sxs-lookup"><span data-stu-id="22d1c-103">If you do not already have App Studio installed in Teams, [install it now](/microsoftteams/platform/concepts/build-and-test/app-studio-overview).</span></span>

1. <span data-ttu-id="22d1c-104">Запустите App Studio в Microsoft Teams и выберите редактор **манифеста.**</span><span class="sxs-lookup"><span data-stu-id="22d1c-104">Launch App Studio in Microsoft Teams and select the **Manifest editor**.</span></span>

1. <span data-ttu-id="22d1c-105">Выберите **"Создать новое приложение"**.</span><span class="sxs-lookup"><span data-stu-id="22d1c-105">Select **Create a new app**.</span></span>

    ![Снимок экрана: редактор манифеста в App Studio в Microsoft Teams](images/app-studio-01.png)

1. <span data-ttu-id="22d1c-107">На странице **сведений о приложении** заполните необходимые поля.</span><span class="sxs-lookup"><span data-stu-id="22d1c-107">On the **App details** page, fill in the required fields.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22d1c-108">Вы можете использовать значки по умолчанию в разделе **"Фирменая** символика" или отправить собственные значки.</span><span class="sxs-lookup"><span data-stu-id="22d1c-108">You can use the default icons in the **Branding** section or upload your own.</span></span>

1. <span data-ttu-id="22d1c-109">В меню слева выберите **"Вкладки"** в меню **"Возможности".**</span><span class="sxs-lookup"><span data-stu-id="22d1c-109">On the left-hand menu, select **Tabs** under **Capabilities**.</span></span>

1. <span data-ttu-id="22d1c-110">Выберите **"Добавить"** **в области "Добавить личную вкладку".**</span><span class="sxs-lookup"><span data-stu-id="22d1c-110">Select **Add** under **Add a personal tab**.</span></span>

    ![Снимок экрана со страницей вкладок в App Studio](images/app-studio-02.png)

1. <span data-ttu-id="22d1c-112">Заполните поля следующим образом, где находится URL-адрес для переадантки, скопированный `YOUR_NGROK_URL` в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="22d1c-112">Fill in the fields as follows, where `YOUR_NGROK_URL` is the forwarding URL you copied in the previous section.</span></span> <span data-ttu-id="22d1c-113">После **этого выберите "Сохранить".**</span><span class="sxs-lookup"><span data-stu-id="22d1c-113">Select **Save** when done.</span></span>

    - <span data-ttu-id="22d1c-114">**Имя:**`Create event`</span><span class="sxs-lookup"><span data-stu-id="22d1c-114">**Name:** `Create event`</span></span>
    - <span data-ttu-id="22d1c-115">**ИД сущности:**`createEventTab`</span><span class="sxs-lookup"><span data-stu-id="22d1c-115">**Entity ID:** `createEventTab`</span></span>
    - <span data-ttu-id="22d1c-116">**URL-адрес контента:**`YOUR_NGROK_URL/newevent`</span><span class="sxs-lookup"><span data-stu-id="22d1c-116">**Content URL:** `YOUR_NGROK_URL/newevent`</span></span>

1. <span data-ttu-id="22d1c-117">Выберите **"Добавить"** **в области "Добавить личную вкладку".**</span><span class="sxs-lookup"><span data-stu-id="22d1c-117">Select **Add** under **Add a personal tab**.</span></span>

1. <span data-ttu-id="22d1c-118">Заполните поля следующим образом, где находится URL-адрес для переадрезки, скопированный `YOUR_NGROK_URL` в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="22d1c-118">Fill in the fields as follows, where `YOUR_NGROK_URL` is the forwarding URL you copied in the previous section.</span></span> <span data-ttu-id="22d1c-119">После **этого выберите "Сохранить".**</span><span class="sxs-lookup"><span data-stu-id="22d1c-119">Select **Save** when done.</span></span>

    - <span data-ttu-id="22d1c-120">**Имя:**`Graph calendar`</span><span class="sxs-lookup"><span data-stu-id="22d1c-120">**Name:** `Graph calendar`</span></span>
    - <span data-ttu-id="22d1c-121">**ИД сущности:**`calendarTab`</span><span class="sxs-lookup"><span data-stu-id="22d1c-121">**Entity ID:** `calendarTab`</span></span>
    - <span data-ttu-id="22d1c-122">**URL-адрес контента:**`YOUR_NGROK_URL`</span><span class="sxs-lookup"><span data-stu-id="22d1c-122">**Content URL:** `YOUR_NGROK_URL`</span></span>

1. <span data-ttu-id="22d1c-123">В меню слева выберите **"Домены" и "Разрешения" в** меню **"Готово".**</span><span class="sxs-lookup"><span data-stu-id="22d1c-123">On the left-hand menu, select **Domains and permissions** under **Finish**.</span></span>

1. <span data-ttu-id="22d1c-124">Задайте для **ИД приложения AAD** ИД приложения из регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="22d1c-124">Set the **AAD App ID** to the application ID from your app registration.</span></span>

1. <span data-ttu-id="22d1c-125">**Задайте в поле единого входе** URI для ИД приложения из регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="22d1c-125">Set the **Single-Sign-On** field to the application ID URI from your app registration.</span></span>

1. <span data-ttu-id="22d1c-126">В меню слева выберите пункт **"Тестирование" и "Распространить" в меню** **"Готово".**</span><span class="sxs-lookup"><span data-stu-id="22d1c-126">On the left-hand menu, select **Test and distribute** under **Finish**.</span></span> <span data-ttu-id="22d1c-127">Выберите **"Скачать"**.</span><span class="sxs-lookup"><span data-stu-id="22d1c-127">Select **Download**.</span></span>

1. <span data-ttu-id="22d1c-128">Создайте каталог в корневой каталог проекта с именем **Manifest.**</span><span class="sxs-lookup"><span data-stu-id="22d1c-128">Create a new directory in the root of the project named **Manifest**.</span></span> <span data-ttu-id="22d1c-129">Извлеките содержимое скачаного ZIP-файла в этот каталог.</span><span class="sxs-lookup"><span data-stu-id="22d1c-129">Extract the contents of the downloaded ZIP file to this directory.</span></span>
