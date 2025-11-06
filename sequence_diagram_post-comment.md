<img width="3553" height="2156" alt="image" src="https://github.com/user-attachments/assets/1cf8f655-e314-41e0-88f4-d61cbc7a3a6c" />


```
title "Оставление комментария POST /task/comment/"

actor user as "Пользователь"

participant apiGateway as "API Gateway"
participant taskService as "Сервис задач"
database taskService_db as "БД сервиса задач"
participant notifService as "Сервис уведомлений"
participant searchService as "Сервис поиска"

user -> apiGateway++: Запрос с текстом комментария
apiGateway -> apiGateway: Аутентификация
apiGateway -> taskService++: Запрос с текстом комментария
taskService -> taskService: Проверка прав доступа к действию

alt case "Проверка прошла успешно"
    taskService -> taskService_db++: Запись комментария
    taskService <- taskService_db--: Подтверждение записи
    taskService -> notifService++: Отправка события
    taskService <- notifService--: Подтверждение
    taskService -> searchService++: Отправка события для переиндексации
    taskService <- searchService--: Подтверждение
    apiGateway <- taskService: Подтверждение записи комментария
    user <- apiGateway: Подтверждение записи комментария
else case "Недосточно прав доступа"
    apiGateway <- taskService--: Отказ в записи комментария - ошибка
    user <- apiGateway--: Отказ в записи комментария - ошибка
end
```
