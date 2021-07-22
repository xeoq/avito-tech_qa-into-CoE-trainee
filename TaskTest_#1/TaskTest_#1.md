## Тестовое задание №1
Схема БД состоит из 2 таблиц:

1. task_status (issue_key, status, start_time)
2. task_info (issue_key, type, name)
Таблица task_status представляет номер задачи в Jira (issue_key), статус задачи (status), время перехода в статус (start_time). Задачи по флоу могут переходить из любого в любой статус (всего их 3 - open, in progress, done). Каждый переход записывается в базу. Таблица task_info представляет номер задачи (issue_key), тип задачи (type), название (name).

### Задание
Перечислите номера задач с их названиями с типом "Bug", которые переходили в статус "in progress" в апреле этого года.
----

### Создаем таблицы
````
CREATE TABLE task_status (
issue_key INTEGER NOT NULL,
status TEXT NOT NULL,
start_time DATETIME NOT NULL
);
CREATE TABLE task_info (
issue_key INTEGER PRIMARY KEY,
type TEXT NOT NULL,
name TEXT NOT NULL
);
````
### Добавляем значения
````
INSERT INTO task_status VALUES (1, 'in progress', '2021-04-01 00:01:01');
INSERT INTO task_status VALUES (4, 'in progress', '2021-04-01 00:01:01');
INSERT INTO task_status VALUES (5, 'open', '2021-04-02 00:01:01');
INSERT INTO task_status VALUES (1, 'done', '2021-04-02 00:01:01');
INSERT INTO task_status VALUES (3, 'in progress', '2021-04-10 15:01:01');
INSERT INTO task_status VALUES (2, 'done', '2022-04-01 00:01:01');

INSERT INTO task_info VALUES (1, 'Bug', 'bug_1');
INSERT INTO task_info VALUES (2, 'Bug', 'bug_2');
INSERT INTO task_info VALUES (3, 'Fix', 'fix_3');
INSERT INTO task_info VALUES (4, 'Bug', 'bug_4');
INSERT INTO task_info VALUES (5, 'Bug', 'bug_5');
````
### Создаем запрос
````
SELECT task_info.issue_key, task_info.name
FROM task_info INNER JOIN task_status
ON task_status.issue_key = task_info.issue_key
WHERE (start_time BETWEEN '2021-04-01 00:00:00' AND '2021-04-30 23:59:59') AND type = 'Bug' AND status = 'in progress';
````
### Полученные значения
| Номер задачи | Название задачи |
| ----------------  | ------------------- |
| 1 | bug_1 |
| 4 | bug_4 |
