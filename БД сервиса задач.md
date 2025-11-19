<img width="1412" height="572" alt="image" src="https://github.com/user-attachments/assets/141e2a83-fadf-4a09-84de-481d116ef4f3" />

```yaml
@startuml

!define Table(name,desc) entity name as "desc" << (T,#FFAAAA) >>
title Сервис задач

Table(Task, "Task (Задачи)") {
  task_id : INT PK
  --
  title: VARCHAR(50) NOT NULL (Название задачи)
  description: TEXT NOT NULL (Описание задачи)
  deadline: TIMESTAMP (Крайний срок выполнения)
}

Table(Tag, "Tag (Теги)") {
  tag_id : INT PK
  --
  name: VARCHAR(20) NOT NULL (Название тега)
  color: VARCHAR(8) NOT NULL DEFAULT "0x000000" (Цвет тега)
}

Table(Task_Tag, "Task_Tag (Связка тега и задачи)") {
  task_id: INT PK FK(Task.task_id) (ID задачи)
  tag_id: INT PK FK(Tag.tag_id) (ID тега)
}

Task ||--o{ Task_Tag : "1:M"
Tag ||--o{ Task_Tag : "1:M"
@enduml
```
