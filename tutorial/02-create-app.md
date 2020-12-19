<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c3499-101">Приложения вкладок Microsoft Teams имеют несколько вариантов проверки подлинности пользователя и вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c3499-101">Microsoft Teams tab applications have multiple options to authenticate the user and call Microsoft Graph.</span></span> <span data-ttu-id="c3499-102">В этом упражнении вы реализуете [](/microsoftteams/platform/tabs/how-to/authentication/auth-aad-sso) вкладку, которая выполняет единый вход для получения [](/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) маркера auth на клиенте, а затем использует поток от имени на сервере для обмена этим маркером для получения доступа к Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c3499-102">In this exercise, you'll implement a tab that does [single sign-on](/microsoftteams/platform/tabs/how-to/authentication/auth-aad-sso) to get an auth token on the client, then uses the [on-behalf-of flow](/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) on the server to exchange that token to get access to Microsoft Graph.</span></span>

<span data-ttu-id="c3499-103">Другие альтернативы см. в следующих вариантах.</span><span class="sxs-lookup"><span data-stu-id="c3499-103">For other alternatives, see the following.</span></span>

- <span data-ttu-id="c3499-104">[Создайте вкладку Microsoft Teams с помощью microsoft Graph набор средств.](/graph/toolkit/get-started/build-a-microsoft-teams-tab)</span><span class="sxs-lookup"><span data-stu-id="c3499-104">[Build a Microsoft Teams tab with the Microsoft Graph Toolkit](/graph/toolkit/get-started/build-a-microsoft-teams-tab).</span></span> <span data-ttu-id="c3499-105">Этот пример полностью на стороне клиента и использует microsoft Graph набор средств для обработки проверки подлинности и вызовов Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c3499-105">This sample is completely client-side, and uses the Microsoft Graph Toolkit to handle authentication and making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="c3499-106">[Пример проверки подлинности Microsoft Teams.](https://github.com/OfficeDev/microsoft-teams-sample-auth-node)</span><span class="sxs-lookup"><span data-stu-id="c3499-106">[Microsoft Teams Authentication Sample](https://github.com/OfficeDev/microsoft-teams-sample-auth-node).</span></span> <span data-ttu-id="c3499-107">В этом примере содержится несколько примеров, охватывающих различные сценарии проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="c3499-107">This sample contains multiple examples covering different authentication scenarios.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="c3499-108">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="c3499-108">Create the project</span></span>

<span data-ttu-id="c3499-109">Для начала создайте ASP.NET Core web app.</span><span class="sxs-lookup"><span data-stu-id="c3499-109">Start by creating an ASP.NET Core web app.</span></span>

1. <span data-ttu-id="c3499-110">Откройте интерфейс командной строки (CLI) в каталоге, где нужно создать проект.</span><span class="sxs-lookup"><span data-stu-id="c3499-110">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="c3499-111">Выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="c3499-111">Run the following command.</span></span>

    ```Shell
    dotnet new webapp -o GraphTutorial
    ```

1. <span data-ttu-id="c3499-112">После создания проекта убедитесь, что он работает, изменив текущий каталог на **каталог GraphTutorial** и выполнив следующую команду в CLI.</span><span class="sxs-lookup"><span data-stu-id="c3499-112">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

1. <span data-ttu-id="c3499-113">Откройте браузер и перейдите к `https://localhost:5001` .</span><span class="sxs-lookup"><span data-stu-id="c3499-113">Open your browser and browse to `https://localhost:5001`.</span></span> <span data-ttu-id="c3499-114">Если все работает, на основной странице должна ASP.NET по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c3499-114">If everything is working, you should see a default ASP.NET Core page.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c3499-115">Если вы получите предупреждение о том, что сертификат **для localhost** не является доверенным, можно использовать .NET Core CLI для установки сертификата разработки и доверия ему.</span><span class="sxs-lookup"><span data-stu-id="c3499-115">If you receive a warning that the certificate for **localhost** is un-trusted you can use the .NET Core CLI to install and trust the development certificate.</span></span> <span data-ttu-id="c3499-116">Инструкции [по определенным операционным системам см.](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) в ASP.NET "Enforce HTTPS in ASP.NET Core".</span><span class="sxs-lookup"><span data-stu-id="c3499-116">See [Enforce HTTPS in ASP.NET Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) for instructions for specific operating systems.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="c3499-117">Добавление пакетов NuGet</span><span class="sxs-lookup"><span data-stu-id="c3499-117">Add NuGet packages</span></span>

<span data-ttu-id="c3499-118">Прежде чем двигаться дальше, установите некоторые дополнительные пакеты NuGet, которые вы будете использовать позже.</span><span class="sxs-lookup"><span data-stu-id="c3499-118">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="c3499-119">[Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) для проверки подлинности и запроса маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="c3499-119">[Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) for authenticating and requesting access tokens.</span></span>
- <span data-ttu-id="c3499-120">[Microsoft.Identity.Web.MicrosoftGraph для](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph/) добавления поддержки Microsoft Graph, настроенной с помощью Microsoft.Identity.Web.</span><span class="sxs-lookup"><span data-stu-id="c3499-120">[Microsoft.Identity.Web.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph/) for adding Microsoft Graph support configured with Microsoft.Identity.Web.</span></span>
- <span data-ttu-id="c3499-121">[Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) для обновления версии этого пакета, установленного с помощью Microsoft.Identity.Web.MicrosoftGraph.</span><span class="sxs-lookup"><span data-stu-id="c3499-121">[Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) to update the version of this package installed by Microsoft.Identity.Web.MicrosoftGraph.</span></span>
- <span data-ttu-id="c3499-122">[Microsoft.AspNetCore.Mvc.NewtonsoftJson,](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) чтобы включить конвертеры Newtonsoft JSON для совместимости с Microsoft Graph SDK.</span><span class="sxs-lookup"><span data-stu-id="c3499-122">[Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) to enable Newtonsoft JSON converters for compatibility with the Microsoft Graph SDK.</span></span>
- <span data-ttu-id="c3499-123">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) для перевода идентификаторов часовой пояс Windows в идентификаторы IANA.</span><span class="sxs-lookup"><span data-stu-id="c3499-123">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) for translating Windows time zone identifiers to IANA identifiers.</span></span>

1. <span data-ttu-id="c3499-124">Чтобы установить зависимости, в командной библиотеке CLI запустите следующие команды.</span><span class="sxs-lookup"><span data-stu-id="c3499-124">Run the following commands in your CLI to install the dependencies.</span></span>

    ```Shell
    dotnet add package Microsoft.Identity.Web --version 1.1.0
    dotnet add package Microsoft.Identity.Web.MicrosoftGraph --version 1.1.0
    dotnet add package Microsoft.Graph --version 3.16.0
    dotnet add package Microsoft.AspNetCore.Mvc.NewtonsoftJson
    dotnet add package TimeZoneConverter
    ```

## <a name="design-the-app"></a><span data-ttu-id="c3499-125">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="c3499-125">Design the app</span></span>

<span data-ttu-id="c3499-126">В этом разделе вы создадим базовую структуру пользовательского интерфейса приложения.</span><span class="sxs-lookup"><span data-stu-id="c3499-126">In this section you will create the basic UI structure of the application.</span></span>

> [!TIP]
> <span data-ttu-id="c3499-127">Для редактирования исходных файлов для этого руководства можно использовать любой текстовый редактор.</span><span class="sxs-lookup"><span data-stu-id="c3499-127">You can use any text editor to edit the source files for this tutorial.</span></span> <span data-ttu-id="c3499-128">Однако Visual Studio [код](https://code.visualstudio.com/) предоставляет дополнительные функции, такие как отладка и Intellisense.</span><span class="sxs-lookup"><span data-stu-id="c3499-128">However, [Visual Studio Code](https://code.visualstudio.com/) provides additional features, such as debugging and Intellisense.</span></span>

1. <span data-ttu-id="c3499-129">Откройте **./Pages/Shared/_Layout.cshtml** и замените все его содержимое следующим кодом для обновления глобального макета приложения.</span><span class="sxs-lookup"><span data-stu-id="c3499-129">Open **./Pages/Shared/_Layout.cshtml** and replace its entire contents with the following code to update the global layout of the app.</span></span>

    :::code language="cshtml" source="../demo/GraphTutorial/Pages/Shared/_Layout.cshtml" id="LayoutSnippet":::

    <span data-ttu-id="c3499-130">Это заменяет Bootstrap на [fluent UI,](https://developer.microsoft.com/fluentui)добавляет [microsoft Teams SDK](/javascript/api/overview/msteams-client)и упрощает макет.</span><span class="sxs-lookup"><span data-stu-id="c3499-130">This replaces Bootstrap with [Fluent UI](https://developer.microsoft.com/fluentui), adds the [Microsoft Teams SDK](/javascript/api/overview/msteams-client), and simplifies the layout.</span></span>

1. <span data-ttu-id="c3499-131">Откройте **./wwwroot/js/site.js** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="c3499-131">Open **./wwwroot/js/site.js** and add the following code.</span></span>

    :::code language="javascript" source="../demo/GraphTutorial/wwwroot/js/site.js" id="ThemeSupportSnippet":::

    <span data-ttu-id="c3499-132">Это добавляет простой обработок изменения темы, чтобы изменить цвет текста по умолчанию для тем тем с высокой контрастности.</span><span class="sxs-lookup"><span data-stu-id="c3499-132">This adds a simple theme change handler to change the default text color for dark and high contrast themes.</span></span>

1. <span data-ttu-id="c3499-133">Откройте **./wwwroot/css/site.css** и замените его содержимое на следующее.</span><span class="sxs-lookup"><span data-stu-id="c3499-133">Open **./wwwroot/css/site.css** and replace its contents with the following.</span></span>

    :::code language="css" source="../demo/GraphTutorial/wwwroot/css/site.css" id="CssSnippet":::

1. <span data-ttu-id="c3499-134">Откройте **./Pages/Index.cshtml** и замените его содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="c3499-134">Open **./Pages/Index.cshtml** and replace its contents with the following code.</span></span>

    ```cshtml
    @page
    @model IndexModel
    @{
      ViewData["Title"] = "Home page";
    }

    <div id="tab-container">
      <h1 class="ms-fontSize-24 ms-fontWeight-semibold">Loading...</h1>
    </div>

    @section Scripts
    {
      <script>
      </script>
    }
    ```

1. <span data-ttu-id="c3499-135">Откройте **./Startup.cs** и `app.UseHttpsRedirection();` удалите строку в `Configure` методе.</span><span class="sxs-lookup"><span data-stu-id="c3499-135">Open **./Startup.cs** and remove the `app.UseHttpsRedirection();` line in the `Configure` method.</span></span> <span data-ttu-id="c3499-136">Это необходимо для работы туннеля ngrok.</span><span class="sxs-lookup"><span data-stu-id="c3499-136">This is necessary for ngrok tunneling to work.</span></span>

## <a name="run-ngrok"></a><span data-ttu-id="c3499-137">Запуск ngrok</span><span class="sxs-lookup"><span data-stu-id="c3499-137">Run ngrok</span></span>

<span data-ttu-id="c3499-138">Microsoft Teams не поддерживает локальное размещение приложений.</span><span class="sxs-lookup"><span data-stu-id="c3499-138">Microsoft Teams does not support local hosting for apps.</span></span> <span data-ttu-id="c3499-139">Сервер, на который размещено приложение, должен быть доступен из облака с помощью конечных точек HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c3499-139">The server hosting your app must be available from the cloud using HTTPS endpoints.</span></span> <span data-ttu-id="c3499-140">Для локальной отладки можно использовать ngrok, чтобы создать общедоступный URL-адрес для локального проекта.</span><span class="sxs-lookup"><span data-stu-id="c3499-140">For debugging locally, you can use ngrok to create a public URL for your locally-hosted project.</span></span>

1. <span data-ttu-id="c3499-141">Откройте CLI и запустите следующую команду, чтобы запустить ngrok.</span><span class="sxs-lookup"><span data-stu-id="c3499-141">Open your CLI and run the following command to start ngrok.</span></span>

    ```Shell
    ngrok http 5000
    ```

1. <span data-ttu-id="c3499-142">После начала ngrok скопируйте URL-адрес переадрит https.</span><span class="sxs-lookup"><span data-stu-id="c3499-142">Once ngrok starts, copy the HTTPS Forwarding URL.</span></span> <span data-ttu-id="c3499-143">Он должен выглядеть как `https://50153897dd4d.ngrok.io` .</span><span class="sxs-lookup"><span data-stu-id="c3499-143">It should look like `https://50153897dd4d.ngrok.io`.</span></span> <span data-ttu-id="c3499-144">Это значение потребуется на последующих шагах.</span><span class="sxs-lookup"><span data-stu-id="c3499-144">You'll need this value in later steps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c3499-145">Если вы используете бесплатную версию ngrok, URL-адрес переадверсии изменяется при каждом перезапуске ngrok.</span><span class="sxs-lookup"><span data-stu-id="c3499-145">If you are using the free version of ngrok, the forwarding URL changes every time you restart ngrok.</span></span> <span data-ttu-id="c3499-146">Рекомендуется оставить ngrok запущенным, пока вы не завершите это руководство, чтобы сохранить тот же URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="c3499-146">It's recommended that you leave ngrok running until you complete this tutorial to keep the same URL.</span></span> <span data-ttu-id="c3499-147">Если вам нужно перезапустить ngrok, вам потребуется обновить URL-адрес везде, где он используется, и переустановить приложение в Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="c3499-147">If you have to restart ngrok, you'll need to update your URL everywhere that it is used and reinstall the app in Microsoft Teams.</span></span>
