<!-- markdownlint-disable MD002 MD041 -->

В этом разделе вы включаете Microsoft Graph в приложение. Для этого приложения вы будете использовать клиентскую библиотеку Microsoft Graph для [.NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) для вызова Microsoft Graph.

# <a name="get-a-calendar-view"></a>Просмотр календаря

Представление календаря — это набор событий из календаря пользователя, которые происходят между двумя точками времени. Он используется для получения событий пользователя за текущую неделю.

1. Откройте **./Controllers/CalendarController.cs** и добавьте следующую функцию в класс **CalendarController.**

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="GetStartOfWeekSnippet":::

1. Добавьте следующую функцию для обработки исключений, возвращаемого вызовами Microsoft Graph.

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="HandleGraphExceptionSnippet":::

1. Замените имеющуюся функцию `Get` указанным ниже кодом.

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="GetSnippet" highlight="2,14-57":::

    Просмотрите изменения. Эта новая версия функции:

    - Возвращает вместо `IEnumerable<Event>` `string` .
    - Получает параметры почтового ящика пользователя с помощью Microsoft Graph.
    - Использует часовой пояс пользователя для расчета начала и окончания текущей недели.
    - Получает представление календаря
        - Использует функцию для включаем загона, что приводит к преобразованию времени начала и окончания возвращаемой события в `.Header()` `Prefer: outlook.timezone` часовой пояс пользователя.
        - Использует `.Top()` функцию для запроса не более 50 событий.
        - Использует `.Select()` функцию для запроса только полей, используемых приложением.
        - Использует `OrderBy()` функцию для сортировки результатов по времени начала.

1. Сохраните изменения и перезапустите приложение. Обновите вкладку в Microsoft Teams. Приложение отображает описание событий в JSON.

## <a name="display-the-results"></a>Отображение результатов

Теперь список событий можно отобразить более удобным способом.

1. Откройте **./Pages/Index.cshtml** и добавьте следующие функции в `<script>` тег.

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="RenderHelpersSnippet":::

1. Замените имеющуюся функцию `renderCalendar` указанным ниже кодом.

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="RenderCalendarSnippet":::

1. Сохраните изменения и перезапустите приложение. Обновите вкладку в Microsoft Teams. Приложение отображает события в календаре пользователя.

    ![Снимок экрана: приложение, отображающие календарь пользователя](images/calendar-view.png)
