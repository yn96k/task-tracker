<img width="6448" height="7380" alt="image" src="https://github.com/user-attachments/assets/c0dc8d96-2ca5-478b-8657-10defb52f74f" />


```plantuml
@startuml "task_tracker_MSA"
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

SHOW_PERSON_OUTLINE()

Person(teammate, "Участник команды", "Выполняет задачи, отчитывается перед руководителем команды")
Person(teamlead, "Руководитель команды", "Ставит задачи, проверяет результат выполнения")

System_Boundary(c1, "Трекер задач") {
    Container(app, "Веб-приложение", "React JS")
    
    Container(api_gateway, "API Gateway", "nginx + OAuth 2.0")

    Container_Boundary(users_b,"Сервис пользователей") {
        Container(users_sv, "Сервис пользователей", "SpringBoot", "Хранит данные о пользователе")
        ContainerDb(users_db, "БД сервиса пользователей", "PostgreSQL")
    }
    Container_Boundary(tasks_b, "Сервис задач") {
        Container(tasks_sv, "Сервис задач", "SpringBoot", "Хранит данные о задачах, сроках, тегах и комментарии")
        ContainerDb(tasks_db, "БД сервиса задач", "PostgreSQL")
    }
    Container_Boundary(search_b, "Сервис поиска") {
        Container(search_sv, "Сервис поиска", "SpringBoot")
        ContainerDb(search_db, "БД сервиса поиска", "Elasticsearch")
    }
    Container_Boundary(files_b, "Сервис вложений") {
        Container(files_sv, "Сервис вложений", "SpringBoot")
        ContainerDb(files_db, "БД сервиса вложений", "MinIO")
    }
    Container_Boundary(notifs_b, "Сервис уведомлений") {
        Container(notifs_sv, "Сервис уведомлений", "SpringBoot")
        ContainerDb(notifs_db, "БД сервиса уведомлений", "PostgreSQL")
    }
}

Rel_D(users_sv, users_db, "")
Rel_D(tasks_sv, tasks_db, "")
Rel_D(search_sv, search_db, "")
Rel_D(files_sv, files_db, "запрашивает файлы при наличии прав доступа")
Rel_D(notifs_sv, notifs_db, "")

Rel(teammate, app, "вносит изменения в задачи, просматривает задачи на исполнении", "https")
Rel(teamlead, app, "ставит задачи и сроки выполнения, оценивает результаты, смотрит метрики", "https")
BiRel(app, api_gateway, "направляет все запросы и получает ответы, передает данные для аутентификации", "https")

BiRel(api_gateway, users_sv, "запрашивает и записывает данные о пользователе", "https")
BiRel(api_gateway, tasks_sv, "запрашивает и записывает данные о задачах", "https")
BiRel(api_gateway, search_sv, "отправляет поисковые запросы и получает сведения", "https")
BiRel(api_gateway, files_sv, "запрашивает и записывает файлы, устанавливает права доступа", "https")
BiRel(api_gateway, notifs_sv, "запрашивает и записывает новые уведомления", "https")

Rel(tasks_sv,search_sv,"сообщает о событиях в БД задач, предоставляет весь текст, комментарии и теги для индексации", "https")
Rel(tasks_sv,notifs_sv,"сообщает о событиях в БД задач, ставит уведомления участникам задачи", "https")

@enduml
```
