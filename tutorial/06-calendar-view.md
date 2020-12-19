<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="95b00-101">В этом разделе вы включаете Microsoft Graph в приложение.</span><span class="sxs-lookup"><span data-stu-id="95b00-101">In this section you will incorporate Microsoft Graph into the application.</span></span> <span data-ttu-id="95b00-102">Для этого приложения вы будете использовать клиентскую библиотеку Microsoft Graph для [.NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="95b00-102">For this application, you will use the [Microsoft Graph Client Library for .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

# <a name="get-a-calendar-view"></a><span data-ttu-id="95b00-103">Просмотр календаря</span><span class="sxs-lookup"><span data-stu-id="95b00-103">Get a calendar view</span></span>

<span data-ttu-id="95b00-104">Представление календаря — это набор событий из календаря пользователя, которые происходят между двумя точками времени.</span><span class="sxs-lookup"><span data-stu-id="95b00-104">A calendar view is a set of events from the user's calendar that occur between two points of time.</span></span> <span data-ttu-id="95b00-105">Он используется для получения событий пользователя за текущую неделю.</span><span class="sxs-lookup"><span data-stu-id="95b00-105">You'll use this to get the user's events for the current week.</span></span>

1. <span data-ttu-id="95b00-106">Откройте **./Controllers/CalendarController.cs** и добавьте следующую функцию в класс **CalendarController.**</span><span class="sxs-lookup"><span data-stu-id="95b00-106">Open **./Controllers/CalendarController.cs** and add the following function to the **CalendarController** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="GetStartOfWeekSnippet":::

1. <span data-ttu-id="95b00-107">Добавьте следующую функцию для обработки исключений, возвращаемого вызовами Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="95b00-107">Add the following function to handle exceptions returned from Microsoft Graph calls.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="HandleGraphExceptionSnippet":::

1. <span data-ttu-id="95b00-108">Замените имеющуюся функцию `Get` указанным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="95b00-108">Replace the existing `Get` function with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="GetSnippet" highlight="2,14-57":::

    <span data-ttu-id="95b00-109">Просмотрите изменения.</span><span class="sxs-lookup"><span data-stu-id="95b00-109">Review the changes.</span></span> <span data-ttu-id="95b00-110">Эта новая версия функции:</span><span class="sxs-lookup"><span data-stu-id="95b00-110">This new version of the function:</span></span>

    - <span data-ttu-id="95b00-111">Возвращает вместо `IEnumerable<Event>` `string` .</span><span class="sxs-lookup"><span data-stu-id="95b00-111">Returns `IEnumerable<Event>` instead of `string`.</span></span>
    - <span data-ttu-id="95b00-112">Получает параметры почтового ящика пользователя с помощью Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="95b00-112">Gets the user's mailbox settings using Microsoft Graph.</span></span>
    - <span data-ttu-id="95b00-113">Использует часовой пояс пользователя для расчета начала и окончания текущей недели.</span><span class="sxs-lookup"><span data-stu-id="95b00-113">Uses the user's time zone to calculate the start and end of the current week.</span></span>
    - <span data-ttu-id="95b00-114">Получает представление календаря</span><span class="sxs-lookup"><span data-stu-id="95b00-114">Gets a calendar view</span></span>
        - <span data-ttu-id="95b00-115">Использует функцию для включаем загона, что приводит к преобразованию времени начала и окончания возвращаемой события в `.Header()` `Prefer: outlook.timezone` часовой пояс пользователя.</span><span class="sxs-lookup"><span data-stu-id="95b00-115">Uses the `.Header()` function to include a `Prefer: outlook.timezone` header, which causes the returned events to have their start and end times converted to the user's timezone.</span></span>
        - <span data-ttu-id="95b00-116">Использует `.Top()` функцию для запроса не более 50 событий.</span><span class="sxs-lookup"><span data-stu-id="95b00-116">Uses the `.Top()` function to request at most 50 events.</span></span>
        - <span data-ttu-id="95b00-117">Использует `.Select()` функцию для запроса только полей, используемых приложением.</span><span class="sxs-lookup"><span data-stu-id="95b00-117">Uses the `.Select()` function to request just the fields used by the app.</span></span>
        - <span data-ttu-id="95b00-118">Использует `OrderBy()` функцию для сортировки результатов по времени начала.</span><span class="sxs-lookup"><span data-stu-id="95b00-118">Uses the `OrderBy()` function to sort the results by the start time.</span></span>

1. <span data-ttu-id="95b00-119">Сохраните изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="95b00-119">Save your changes and restart the app.</span></span> <span data-ttu-id="95b00-120">Обновите вкладку в Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="95b00-120">Refresh the tab in Microsoft Teams.</span></span> <span data-ttu-id="95b00-121">Приложение отображает описание событий в JSON.</span><span class="sxs-lookup"><span data-stu-id="95b00-121">The app displays a JSON listing of the events.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="95b00-122">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="95b00-122">Display the results</span></span>

<span data-ttu-id="95b00-123">Теперь список событий можно отобразить более удобным способом.</span><span class="sxs-lookup"><span data-stu-id="95b00-123">Now you can display the list of events in a more user friendly way.</span></span>

1. <span data-ttu-id="95b00-124">Откройте **./Pages/Index.cshtml** и добавьте следующие функции в `<script>` тег.</span><span class="sxs-lookup"><span data-stu-id="95b00-124">Open **./Pages/Index.cshtml** and add the following functions inside the `<script>` tag.</span></span>

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="RenderHelpersSnippet":::

1. <span data-ttu-id="95b00-125">Замените имеющуюся функцию `renderCalendar` указанным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="95b00-125">Replace the existing `renderCalendar` function with the following.</span></span>

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="RenderCalendarSnippet":::

1. <span data-ttu-id="95b00-126">Сохраните изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="95b00-126">Save your changes and restart the app.</span></span> <span data-ttu-id="95b00-127">Обновите вкладку в Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="95b00-127">Refresh the tab in Microsoft Teams.</span></span> <span data-ttu-id="95b00-128">Приложение отображает события в календаре пользователя.</span><span class="sxs-lookup"><span data-stu-id="95b00-128">The app displays events on the user's calendar.</span></span>

    ![Снимок экрана: приложение, отображающие календарь пользователя](images/calendar-view.png)
