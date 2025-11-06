<img width="2953" height="818" alt="image_2025-11-06_22-33-29" src="https://github.com/user-attachments/assets/730f5111-306c-4ef7-8808-2bf5457bf37b" />

```
@startuml

!define Table(name,desc) entity name as "desc" << (T,#FFAAAA) >>
title Сервис задач

Table(Task, "Task (Задачи)") {
  task_id : INT PK
  --
  supertask_id: INT (ID задачи верхнего уровня - для подзадач)
  title: VARCHAR(50) NOT NULL (Название задачи)
  description: TEXT NOT NULL (Описание задачи)
  teamlead_id: INT NOT NULL (ID постановщика задачи)
  assignee_id: INT NOT NULL (ID исполнителя)
  status: ENUM(idle, in_progress, pause, awaits, done) NOT NULL DEFAULT 'idle' (Статус задачи - оиждает выполнения, выполняется, на паузе, ждет обратной связи, выполнена)
  deadline: TIMESTAMP (Крайний срок выполнения)
}

Table(Comment, "Comment (Комментарии)") {
  comment_id : INT PK
  --
  task_id: INT NOT NULL FK(Task.task_id) (ID задачи)
  text: VARCHAR(50) NOT NULL (Текст комментария)
  author_id: INT NOT NULL (ID автора комментария)
  is_result: BOOLEAN NOT NULL DEFAULT FALSE (Является ли комментарий результатом)
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

Task ||--o{ Comment : "1:M"
Task ||--o{ Task_Tag : "1:M"
Tag ||--o{ Task_Tag : "1:M"
@enduml
```
