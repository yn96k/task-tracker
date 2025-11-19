<img width="1983" height="3680" alt="image" src="https://github.com/user-attachments/assets/8ddb4489-be0b-43b9-a278-a795c932c2ec" />


```plantuml
@startuml "task_tracker_MSA"
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

SHOW_PERSON_OUTLINE()

Person(user, "Пользователь", "Манипулирует своими задачами, ставит и получает уведомления")

System_Boundary(c1, "Трекер задач") {
    Container(app, "Веб-приложение", "React JS")
    
    Container(api_gateway, "API Gateway", "nginx + OAuth 2.0")

    Container_Boundary(users_b,"Сервис пользователей") {
        Container(users_sv, "Сервис пользователей", "SpringBoot", "Хранит данные о пользователях")
        ContainerDb(users_db, "БД сервиса пользователей", "PostgreSQL")
    }
    Container_Boundary(tasks_b, "Сервис задач") {
        Container(tasks_sv, "Сервис задач", "SpringBoot", "Хранит данные о задачах, сроках и тегах")
        ContainerDb(tasks_db, "БД сервиса задач", "PostgreSQL")
    }
    Container_Boundary(notifs_b, "Сервис уведомлений") {
        Container(notifs_sv, "Сервис уведомлений", "SpringBoot")
        ContainerDb(notifs_db, "БД сервиса уведомлений", "PostgreSQL")
    }
}

Rel_D(users_sv, users_db, "")
Rel_D(tasks_sv, tasks_db, "")
Rel_D(notifs_sv, notifs_db, "")

Rel(user, app, "вносит изменения в задачи, просматривает задачи, получает уведомления", "https")
BiRel(app, api_gateway, "направляет все запросы и получает ответы, передает данные для аутентификации", "https")

BiRel(api_gateway, users_sv, "запрашивает и записывает данные о пользователе", "https")
BiRel(api_gateway, tasks_sv, "запрашивает и записывает данные о задачах", "https")
BiRel(api_gateway, notifs_sv, "запрашивает и записывает новые уведомления", "https")

Rel(tasks_sv,notifs_sv,"сообщает о событиях в БД задач, ставит уведомления участникам задачи", "https")

@enduml
```
