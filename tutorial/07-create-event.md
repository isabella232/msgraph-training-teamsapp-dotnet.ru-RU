<!-- markdownlint-disable MD002 MD041 -->

В этом разделе вы добавим возможность создания событий в календаре пользователя.

## <a name="create-the-new-event-tab"></a>Создание новой вкладки события

1. Создайте файл **в каталоге ./Pages** с именем **NewEvent.cshtml** и добавьте следующий код.

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.cshtml":::

    Это реализует простую форму и добавляет JavaScript для публикации данных формы в веб-API.

## <a name="implement-the-web-api"></a>Реализация веб-API

1. Создайте каталог с именем **Models** в корневой каталог проекта.

1. Создайте новый файл в **каталоге ./Models** **с именем NewEvent.cs** и добавьте следующий код.

    :::code language="csharp" source="../demo/GraphTutorial/Models/NewEvent.cs" id="NewEventSnippet":::

1. Откройте **файл ./Controllers/CalendarController.cs** и добавьте следующую строку в `using` верхней части файла.

    ```csharp
    using GraphTutorial.Models;
    ```

1. Добавьте в класс **CalendarController** следующую функцию.

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="PostSnippet":::

    Это позволяет http POST веб-API с полями формы.

1. Сохраните все изменения и перезапустите приложение. Обновите приложение в Microsoft Teams и выберите вкладку **"Создать событие".** Заполните форму и выберите **"Создать",** чтобы добавить событие в календарь пользователя.

    ![Снимок экрана: вкладка "Создание события"](images/create-event.png)
