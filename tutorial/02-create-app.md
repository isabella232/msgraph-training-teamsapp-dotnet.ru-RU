<!-- markdownlint-disable MD002 MD041 -->

Приложения вкладок Microsoft Teams имеют несколько вариантов проверки подлинности пользователя и вызова Microsoft Graph. В этом упражнении вы реализуете [](/microsoftteams/platform/tabs/how-to/authentication/auth-aad-sso) вкладку, которая выполняет единый вход для получения [](/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) маркера auth на клиенте, а затем использует поток от имени на сервере для обмена этим маркером для получения доступа к Microsoft Graph.

Другие альтернативы см. в следующих вариантах.

- [Создайте вкладку Microsoft Teams с помощью microsoft Graph набор средств.](/graph/toolkit/get-started/build-a-microsoft-teams-tab) Этот пример полностью на стороне клиента и использует microsoft Graph набор средств для обработки проверки подлинности и вызовов Microsoft Graph.
- [Пример проверки подлинности Microsoft Teams.](https://github.com/OfficeDev/microsoft-teams-sample-auth-node) В этом примере содержится несколько примеров, охватывающих различные сценарии проверки подлинности.

## <a name="create-the-project"></a>Создание проекта

Для начала создайте ASP.NET Core web app.

1. Откройте интерфейс командной строки (CLI) в каталоге, где нужно создать проект. Выполните следующую команду.

    ```Shell
    dotnet new webapp -o GraphTutorial
    ```

1. После создания проекта убедитесь, что он работает, изменив текущий каталог на **каталог GraphTutorial** и выполнив следующую команду в CLI.

    ```Shell
    dotnet run
    ```

1. Откройте браузер и перейдите к `https://localhost:5001` . Если все работает, на основной странице должна ASP.NET по умолчанию.

> [!IMPORTANT]
> Если вы получите предупреждение о том, что сертификат **для localhost** не является доверенным, можно использовать .NET Core CLI для установки сертификата разработки и доверия ему. Инструкции [по определенным операционным системам см.](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) в ASP.NET "Enforce HTTPS in ASP.NET Core".

## <a name="add-nuget-packages"></a>Добавление пакетов NuGet

Прежде чем двигаться дальше, установите некоторые дополнительные пакеты NuGet, которые вы будете использовать позже.

- [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) для проверки подлинности и запроса маркеров доступа.
- [Microsoft.Identity.Web.MicrosoftGraph для](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph/) добавления поддержки Microsoft Graph, настроенной с помощью Microsoft.Identity.Web.
- [Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) для обновления версии этого пакета, установленного с помощью Microsoft.Identity.Web.MicrosoftGraph.
- [Microsoft.AspNetCore.Mvc.NewtonsoftJson,](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) чтобы включить конвертеры Newtonsoft JSON для совместимости с Microsoft Graph SDK.
- [TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) для перевода идентификаторов часовой пояс Windows в идентификаторы IANA.

1. Чтобы установить зависимости, в командной библиотеке CLI запустите следующие команды.

    ```Shell
    dotnet add package Microsoft.Identity.Web --version 1.1.0
    dotnet add package Microsoft.Identity.Web.MicrosoftGraph --version 1.1.0
    dotnet add package Microsoft.Graph --version 3.16.0
    dotnet add package Microsoft.AspNetCore.Mvc.NewtonsoftJson
    dotnet add package TimeZoneConverter
    ```

## <a name="design-the-app"></a>Проектирование приложения

В этом разделе вы создадим базовую структуру пользовательского интерфейса приложения.

> [!TIP]
> Для редактирования исходных файлов для этого руководства можно использовать любой текстовый редактор. Однако Visual Studio [код](https://code.visualstudio.com/) предоставляет дополнительные функции, такие как отладка и Intellisense.

1. Откройте **./Pages/Shared/_Layout.cshtml** и замените все его содержимое следующим кодом для обновления глобального макета приложения.

    :::code language="cshtml" source="../demo/GraphTutorial/Pages/Shared/_Layout.cshtml" id="LayoutSnippet":::

    Это заменяет Bootstrap на [fluent UI,](https://developer.microsoft.com/fluentui)добавляет [microsoft Teams SDK](/javascript/api/overview/msteams-client)и упрощает макет.

1. Откройте **./wwwroot/js/site.js** и добавьте следующий код.

    :::code language="javascript" source="../demo/GraphTutorial/wwwroot/js/site.js" id="ThemeSupportSnippet":::

    Это добавляет простой обработок изменения темы, чтобы изменить цвет текста по умолчанию для тем тем с высокой контрастности.

1. Откройте **./wwwroot/css/site.css** и замените его содержимое на следующее.

    :::code language="css" source="../demo/GraphTutorial/wwwroot/css/site.css" id="CssSnippet":::

1. Откройте **./Pages/Index.cshtml** и замените его содержимое следующим кодом.

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

1. Откройте **./Startup.cs** и `app.UseHttpsRedirection();` удалите строку в `Configure` методе. Это необходимо для работы туннеля ngrok.

## <a name="run-ngrok"></a>Запуск ngrok

Microsoft Teams не поддерживает локальное размещение приложений. Сервер, на который размещено приложение, должен быть доступен из облака с помощью конечных точек HTTPS. Для локальной отладки можно использовать ngrok, чтобы создать общедоступный URL-адрес для локального проекта.

1. Откройте CLI и запустите следующую команду, чтобы запустить ngrok.

    ```Shell
    ngrok http 5000
    ```

1. После начала ngrok скопируйте URL-адрес переадрит https. Он должен выглядеть как `https://50153897dd4d.ngrok.io` . Это значение потребуется на последующих шагах.

> [!IMPORTANT]
> Если вы используете бесплатную версию ngrok, URL-адрес переадверсии изменяется при каждом перезапуске ngrok. Рекомендуется оставить ngrok запущенным, пока вы не завершите это руководство, чтобы сохранить тот же URL-адрес. Если вам нужно перезапустить ngrok, вам потребуется обновить URL-адрес везде, где он используется, и переустановить приложение в Microsoft Teams.
