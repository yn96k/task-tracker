<img width="3830" height="2368" alt="image" src="https://github.com/user-attachments/assets/f72aa18e-074a-4c24-a491-0353369f1b3c" />

```
title "Оставление комментария POST /task/comment/"

actor user as "Пользователь"

participant apiGateway as "API Gateway"
participant taskService as "Сервис задач"
database taskService_db as "БД сервиса задач"
participant notifService as "Сервис уведомлений"

user -> apiGateway++: Запрос с текстом комментария
apiGateway -> apiGateway: Аутентификация
apiGateway -> taskService++: Запрос с текстом комментария
taskService -> taskService: Проверка прав доступа к действию

alt case "Проверка прошла успешно"
    taskService -> taskService_db++: Запись комментария
    taskService <- taskService_db--: Подтверждение записи
    taskService -> notifService++: Отправка события
    taskService <- notifService--: Подтверждение
    apiGateway <- taskService: Подтверждение записи комментария
    user <- apiGateway: Подтверждение записи комментария
else case "Недосточно прав доступа"
    apiGateway <- taskService--: Отказ в записи комментария - ошибка
    user <- apiGateway--: Отказ в записи комментария - ошибка
end
```
