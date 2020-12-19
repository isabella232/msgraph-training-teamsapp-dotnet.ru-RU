<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы расширим приложение из предыдущего упражнения, чтобы поддерживать проверку подлинности с единым входом в Azure AD. Это необходимо для получения необходимого маркера доступа OAuth для вызова API Microsoft Graph. На этом этапе вы настроим [библиотеку Microsoft.Identity.Web.](https://www.nuget.org/packages/Microsoft.Identity.Web/)

> [!IMPORTANT]
> Чтобы не хранить код и секрет приложения в источнике, для хранения этих значений используется диспетчер секретов [.NET.](/aspnet/core/security/app-secrets) Диспетчер секретов предназначен только для разработки, в производственных приложениях для хранения секретов должен использовать доверенный диспетчер секретов.

1. Откройте **./appsettings.jsи** замените его содержимое на следующее.

    :::code language="json" source="../demo/GraphTutorial/appsettings.example.json" highlight="2-8":::

1. Откройте CLI в каталоге, в котором расположен **GraphTutorial.csproj,** и запустите следующие команды, заменив его на ИД приложения на портале Azure и с помощью секрета `YOUR_APP_ID` `YOUR_APP_SECRET` приложения.

    ```Shell
    dotnet user-secrets init
    dotnet user-secrets set "AzureAd:ClientId" "YOUR_APP_ID"
    dotnet user-secrets set "AzureAd:ClientSecret" "YOUR_APP_SECRET"
    ```

## <a name="implement-sign-in"></a>Реализация входов

Сначала реализуем единый вход в коде JavaScript приложения. Вы будете использовать [SDK JavaScript](/javascript/api/overview/msteams-client) для Microsoft Teams, чтобы получить маркер доступа, который позволяет коду JavaScript, запущенным в клиенте Teams, выполнять вызовы AJAX к веб-API, которые будут реализованы позже.

1. Откройте **./Pages/Index.cshtml** и добавьте следующий код в `<script>` тег.

    ```javascript
    (function () {
      if (microsoftTeams) {
        microsoftTeams.initialize();

        microsoftTeams.authentication.getAuthToken({
          successCallback: (token) => {
            // TEMPORARY: Display the access token for debugging
            $('#tab-container').empty();

            $('<code/>', {
              text: token,
              style: 'word-break: break-all;'
            }).appendTo('#tab-container');
          },
          failureCallback: (error) => {
            renderError(error);
          }
        });
      }
    })();

    function renderError(error) {
      $('#tab-container').empty();

      $('<h1/>', {
        text: 'Error'
      }).appendTo('#tab-container');

      $('<code/>', {
        text: JSON.stringify(error, Object.getOwnPropertyNames(error)),
        style: 'word-break: break-all;'
      }).appendTo('#tab-container');
    }
    ```

    При этом вызывается автоматический вызов проверки подлинности от пользователя, который `microsoftTeams.authentication.getAuthToken` въехает в Teams. Обычно запросы пользовательского интерфейса не используются, если пользователю не нужно согласиться. Затем код отображает маркер на вкладке.

1. Сохраните изменения и запустите приложение, выполив следующую команду в CLI.

    ```Shell
    dotnet run
    ```

    > [!IMPORTANT]
    > Если вы перезапустили ngrok и ВАШ URL-адрес ngrok изменился, перед  тестированием обязательно обновите значение ngrok в следующем месте.
    >
    > - URI перенаправления при регистрации приложения
    > - URI ИД приложения при регистрации приложения
    > - `contentUrl` в manifest.jsв
    > - `validDomains` в manifest.jsв
    > - `resource` в manifest.jsв

1. Создайте ZIP-файл **сmanifest.js,** **color.png** **и** outline.png.

1. В Microsoft Teams выберите **"Приложения"** в левой панели, выберите "Отправить пользовательское приложение", а затем выберите "Отправить для меня или **моих команд".**

    ![Снимок экрана: ссылка "Отправка настраиваемого приложения" в Microsoft Teams](images/upload-custom-app.png)

1. Перейдите к РАНЕЕ созданному ZIP-файлу и выберите **"Открыть".**

1. Просмотрите сведения о приложении и выберите **"Добавить".**

1. Приложение откроется в Teams и отобразит маркер доступа.

Если вы скопируете маркер, его можно в [jwt.ms.](https://jwt.ms) Убедитесь, что аудитория (утверждение) — это ваш ИД приложения, а единственная область (утверждение) — это созданная `aud` `scp` вами область `access_as_user` API. Это означает, что этот маркер не предоставляет прямой доступ к Microsoft Graph! Вместо этого [веб-API,](/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) который будет реализован в ближайшее время, должен будет обменяться этим маркером с помощью потока "от имени", чтобы получить маркер, который будет работать с вызовами Microsoft Graph.

## <a name="configure-authentication-in-the-aspnet-core-app"></a>Настройка проверки подлинности в приложении ASP.NET Core

Для начала добавьте службы платформы удостоверений Майкрософт в приложение.

1. Откройте файл **./Startup.cs** и добавьте в верхнюю часть файла следующий `using` выписку.

    ```csharp
    using Microsoft.Identity.Web;
    ```

1. Добавьте следующую строку перед `app.UseAuthorization();` строкой в `Configure` функции.

    ```csharp
    app.UseAuthentication();
    ```

1. Добавьте следующую строку сразу `endpoints.MapRazorPages();` после строки в `Configure` функции.

    ```csharp
    endpoints.MapControllers();
    ```

1. Замените имеющуюся функцию `ConfigureServices` указанным ниже кодом.

    :::code language="csharp" source="../demo/GraphTutorial/Startup.cs" id="ConfigureServicesSnippet":::

    Этот код настраивает приложение, чтобы разрешить проверку подлинности вызовов веб-API на основе маркера носитель JWT в `Authorization` заголе. Он также добавляет службы получения маркеров, которые могут обмениваться этим маркером через поток "от имени".

## <a name="create-the-web-api-controller"></a>Создание контроллера веб-API

1. Создайте каталог в корне проекта с именем **Controllers.**

1. Создайте новый файл в **каталоге ./Controllers** с именем CalendarController.cs и **добавьте** следующий код.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Net;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Authorization;
    using Microsoft.AspNetCore.Http;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Logging;
    using Microsoft.Identity.Web;
    using Microsoft.Identity.Web.Resource;
    using Microsoft.Graph;
    using TimeZoneConverter;

    namespace GraphTutorial.Controllers
    {
        [ApiController]
        [Route("[controller]")]
        [Authorize]
        public class CalendarController : ControllerBase
        {
            private static readonly string[] apiScopes = new[] { "access_as_user" };

            private readonly GraphServiceClient _graphClient;
            private readonly ITokenAcquisition _tokenAcquisition;
            private readonly ILogger<CalendarController> _logger;

            public CalendarController(ITokenAcquisition tokenAcquisition, GraphServiceClient graphClient, ILogger<CalendarController> logger)
            {
                _tokenAcquisition = tokenAcquisition;
                _graphClient = graphClient;
                _logger = logger;
            }

            [HttpGet]
            public async Task<string> Get()
            {
                // This verifies that the access_as_user scope is
                // present in the bearer token, throws if not
                HttpContext.VerifyUserHasAnyAcceptedScope(apiScopes);

                // To verify that the identity libraries have authenticated
                // based on the token, log the user's name
                _logger.LogInformation($"Authenticated user: {User.GetDisplayName()}");

                try
                {
                    // TEMPORARY
                    // Get a Graph token via OBO flow
                    var token = await _tokenAcquisition
                        .GetAccessTokenForUserAsync(new[]{
                            "User.Read",
                            "MailboxSettings.Read",
                            "Calendars.ReadWrite" });

                    // Log the token
                    _logger.LogInformation($"Access token for Graph: {token}");
                    return "{ \"status\": \"OK\" }";
                }
                catch (MicrosoftIdentityWebChallengeUserException ex)
                {
                    _logger.LogError(ex, "Consent required");
                    // This exception indicates consent is required.
                    // Return a 403 with "consent_required" in the body
                    // to signal to the tab it needs to prompt for consent
                    HttpContext.Response.ContentType = "text/plain";
                    HttpContext.Response.StatusCode = (int)HttpStatusCode.Forbidden;
                    await HttpContext.Response.WriteAsync("consent_required");
                    return null;
                }
                catch (Exception ex)
                {
                    _logger.LogError(ex, "Error occurred");
                    return null;
                }
            }
        }
    }
    ```

    При этом реализуется веб-API ( ), который `GET /calendar` может быть вызван на вкладке Teams. Пока он просто пытается обменять маркер носителя на маркер Graph. При первой загрузке вкладки происходит сбой, так как пользователь еще не разрешил приложению доступ к Microsoft Graph от его имени.

1. Откройте **./Pages/Index.cshtml** и замените `successCallback` функцию на следующую.

    ```javascript
    successCallback: (token) => {
      // TEMPORARY: Call the Web API
      fetch('/calendar', {
        headers: {
          'Authorization': `Bearer ${token}`
        }
      }).then(response => {
        response.text()
          .then(body => {
            $('#tab-container').empty();
            $('<code/>', {
              text: body
            }).appendTo('#tab-container');
          });
      }).catch(error => {
        console.error(error);
        renderError(error);
      });
    }
    ```

    При этом будет вызываться веб-API и отображаться ответ.

1. Сохраните изменения и перезапустите приложение. Обновите вкладку в Microsoft Teams. Должна отобразиться `consent_required` страница.

1. Просмотрите выходные данные журнала в CLI. Обратите внимание на две вещи.

    - Запись типа `Authenticated user: MeganB@contoso.com` . Веб-API аутентилировал пользователя на основе маркера, отправленного с помощью запроса API.
    - Запись типа `AADSTS65001: The user or administrator has not consented to use the application with ID...` . Это ожидаемо, так как пользователю еще не было предложено согласиться на запрашиваемую область разрешений Microsoft Graph.

## <a name="implement-consent-prompt"></a>Реализация запроса согласия

Так как веб-API не может подсказить пользователю, на вкладке Teams необходимо реализовать запрос. Это необходимо сделать только один раз для каждого пользователя. После того как пользователь дает согласие, он не должен повторно согласиться, если он явно не отозовет доступ к вашему приложению.

1. Создайте файл в **каталоге ./Pages** с именем Authenticate.cshtml.cs и **добавьте** следующий код.

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Authenticate.cshtml.cs" id="AuthenticateModelSnippet":::

1. Создайте файл в **каталоге ./Pages** **с именем Authenticate.cshtml** и добавьте следующий код.

    :::code language="razor" source="../demo/GraphTutorial/Pages/Authenticate.cshtml":::

1. Создайте файл **в каталоге ./Pages** с именем **AuthComplete.cshtml** и добавьте следующий код.

    :::code language="razor" source="../demo/GraphTutorial/Pages/AuthComplete.cshtml":::

1. Откройте **./Pages/Index.cshtml** и добавьте следующие функции в `<script>` тег.

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="LoadUserCalendarSnippet":::

1. Добавьте в тег следующую функцию, чтобы отобразить `<script>` успешный результат из веб-API.

    ```javascript
    function renderCalendar(events) {
      $('#tab-container').empty();

      $('<pre/>').append($('<code/>', {
        text: JSON.stringify(events, null, 2),
        style: 'word-break: break-all;'
      })).appendTo('#tab-container');
    }
    ```

1. Замените `successCallback` существующий следующим кодом.

    ```javascript
    successCallback: (token) => {
      loadUserCalendar(token, (events) => {
        renderCalendar(events);
      });
    }
    ```

1. Сохраните изменения и перезапустите приложение. Обновите вкладку в Microsoft Teams. Вкладка должна `{ "status": "OK" }` отображаться.

1. Просмотрите выходные данные журнала. Должна отлиться `Access token for Graph` запись. Если вы разберите этот маркер, вы заметите, что он содержит области Microsoft Graph, настроенные вappsettings.js **on.**

## <a name="storing-and-refreshing-tokens"></a>Хранение и обновление маркеров

На этом этапе приложение имеет маркер доступа, который отправляется в `Authorization` заголовке вызовов API. Это маркер, который позволяет приложению получать доступ к Microsoft Graph от имени пользователя.

Однако этот маркер является кратковременной. Срок действия маркера истекает через час после его выпуска. В этом случае маркер обновления становится полезным. Маркер обновления позволяет приложению запрашивать новый маркер доступа, не требуя от пользователя повторного входить.

Так как приложение использует библиотеку Microsoft.Identity.Web, вам не нужно реализовывать логику хранения маркеров или обновления.

Приложение использует кэш маркеров в памяти, которого достаточно для приложений, которым не нужно сохранять маркеры при перезапуске приложения. Вместо этого производственные приложения могут использовать [параметры распределенного кэша](https://github.com/AzureAD/microsoft-identity-web/wiki/token-cache-serialization) в библиотеке Microsoft.Identity.Web.

Метод `GetAccessTokenForUserAsync` обрабатывает истечение срока действия маркера и обновление. Сначала он проверяет кэшный маркер и, если срок его действия еще не истек, возвращает его. Если срок действия истек, для получения нового маркера используется кэшный маркер обновления.

**GraphServiceClient,** который получают контроллеры с помощью вливания зависимостей, предварительно настраивается с помощью поставщика проверки подлинности, который `GetAccessTokenForUserAsync` используется для вас.
