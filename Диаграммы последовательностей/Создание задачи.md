<img width="1595" height="926" alt="image" src="https://github.com/user-attachments/assets/a59099f7-07ed-4648-af27-35f89e3eddbe" />




```plantuml
title "Создание задачи POST /task/"

actor user as "Пользователь"

participant apiGateway as "API Gateway"
participant taskService as "Сервис задач"
database taskService_db as "БД сервиса задач"
participant notif_service as "Сервис уведомлений"

user -> apiGateway++: Запрос на создание задачи
apiGateway -> apiGateway: Аутентификация

apiGateway -> taskService++: Запрос на создание задачи
taskService -> taskService_db++: Попытка записи в базу данных

alt case "Запись в БД прошла успешно"
taskService <- taskService_db: Запись прошла успешно

opt "Задача с напоминанием"
    taskService -> notif_service++: Запись уведомления в очередь
    taskService <- notif_service--: Ответ о статусе записи
end

apiGateway <- taskService: Запись прошла успешно + статус напоминания
user <- apiGateway: Запись прошла успешно + статус напоминания
else "Запись в БД не удалась"
taskService <- taskService_db--: Ошибка записи в БД
apiGateway <- taskService: Ошибка создания задачи
user <- apiGateway--: Ошибка создания задачи
end
```
