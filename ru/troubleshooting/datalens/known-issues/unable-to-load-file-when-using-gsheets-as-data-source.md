# Проблема при создании подключения к таблице Google Sheets из DataLens

## Описание проблемы {#issue-description} 
Не получается создать подключение к Google Sheets – после указания ссылки на документ и длительного ожидания отображается сообщение об ошибке:
```
Не удалось загрузить файл
Request ID: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXX
``` 

# Решение {#issue-resolution}
Проверьте, попадает ли ваша таблица Google Sheets под один или несколько указанных ниже критериев:
* Превышен лимит в 200 МБ итоговых данных для каждого листа таблицы Google Sheets;
* Превышен лимит в 300 колонок на одном листе таблицы Google Sheets;
* Хотя бы на одном из листов таблицы присутствует лишь одна заполненная строка. Строк с данными должно быть как минимум две, поскольку столбцы первой строки используются в качестве заголовков полей датасета;
* Наименование таблицы Google Sheets содержит знаки `+` или `/`.

Если таблица Google Sheets, к которой вы пытаетесь настроить подключение, под один или несколько указанных критериев, создайте копию этой таблицы и отредактируйте ее с учетом лимитов и особенностей сервиса DataLens.

## Если проблема осталась {#if-issue-still-persists}
Если вышеописанные действия не помогли решить проблему, [создайте запрос в техническую поддержку](https://console.cloud.yandex.ru/support?section=contact).
В запросе укажите следующую информацию:
1. Полный текст сообщения об ошибке с Request ID;
2. [HAR-файл](../../../support/create-har.md) с сохраненными результатами взаимодействия браузера и серверов DataLens.
