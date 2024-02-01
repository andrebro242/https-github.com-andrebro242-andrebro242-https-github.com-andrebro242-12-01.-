Домашнее задание по лекции "Базы данных" Брюхов А SYS-26

Легенда
Заказчик передал вам файл в формате Excel , в котором сформирован отчёт.

На основе этой отчёта необходимо выполнить следующее задание.

Задание 1
Запишите не менее семи таблиц, из которых состоит база данных:

какие данные хранятся в этих таблицах;
Какой тип данных у столбцов в этих таблицах, если данные хранятся в PostgreSQL.
Приняв решение к следующему мнению:

Сотрудники (

идентификатор, первичный ключ, серийный номер,
фамилия варчар(50),
...
идентификатор структурного подразделения, внешний ключ, целое число).

Решение 1

1.Таблица "Сотрудники"

    CREATE TABLE Сотрудники (

    Идентификатор SERIAL PRIMARY KEY,

    Фамилия VARCHAR(50),

    Имя VARCHAR(50),

    Отчество VARCHAR(50),

    Оклад REAL,

    Должность VARCHAR(50),

    Дата_найма DATE,

    Адрес_филиала VARCHAR(255),

    Проект_назначения VARCHAR(50),

    Идентификатор_структурного_подразделения INTEGER REFERENCES Структурные_подразделения(Идентификатор)

    );


2.Таблица "Структурные подразделения"

    CREATE TABLE Структурные_подразделения (

    Идентификатор SERIAL PRIMARY KEY,

    Название_подразделения VARCHAR(50)

    Идентификатор_адреса INTEGER REFERENCES Адреса_филиалов(Идентификатор)

    );

3.Таблица "Проекты"

    CREATE TABLE Проекты (

    Идентификатор SERIAL PRIMARY KEY,

    Название_проекта VARCHAR(50)

    );

4.Таблица "Типы подразделений"

    CREATE TABLE Типы_подразделений (

    Идентификатор SERIAL PRIMARY KEY,

    Тип_подразделения VARCHAR(50)

    );

5.Таблица "Адреса филиалов"

    CREATE TABLE Адреса_филиалов (

    Идентификатор SERIAL PRIMARY KEY,

    Адрес VARCHAR(255)

    );

6.Таблица "Должности"

    CREATE TABLE Должности (

    Идентификатор SERIAL PRIMARY KEY,

    Название_должности VARCHAR(50)

    );

7.Таблица "Оклады"

    CREATE TABLE Оклады (

    Идентификатор SERIAL PRIMARY KEY,

    Оклад REAL,

    Идентификатор_сотрудника INTEGER REFERENCES Сотрудники(Идентификатор)

    );

Типы данных в PostgreSQL могут быть примерно следующими:

    целое число - integer

    вещественное число - real или numeric

    фамилия и название - varchar(50)

Решение 2

Функциональные зависимости в таблице "Сотрудники":
Ф.И.О сотрудника -> Идентификатор (полное имя сотрудника зависит от идентификатора)
Идентификатор структурного подразделения -> Тип подразделения, Структурное подразделение (зависимость от идентификатора структурного подразделения)

Многозначные зависимости в таблице "Сотрудники":
Идентификатор -> Ф.И.О сотрудника, Оклад, Должность, Тип подразделения, Структурное подразделение, Дата найма, Адрес филиала, Проект на который назначен

Правила нормализации данных:

Первая нормальная форма (1NF):
Все значения в столбцах должны быть атомарными.
Например, можно разделить столбец "Ф.И.О сотрудника" на отдельные столбцы "Фамилия", "Имя", "Отчество".

Вторая нормальная форма (2NF):
Данные должны быть в 1NF, и все неключевые столбцы должны зависеть от полного первичного ключа.
Можно выделить таблицы "Структурные подразделения", "Проекты", "Типы подразделений", "Адреса филиалов", "Должности", "Дата найма" и использовать соответствующие внешние ключи в таблице "Сотрудники".

Третья нормальная форма (3NF):
Данные должны быть в 2NF, и все неключевые столбцы должны быть функционально зависимы от первичного ключа, но не от других неключевых столбцов.
Например, можно выделить таблицу "Зарплаты" для хранения данных об окладах сотрудников.

Нормальная форма Бойса-Кодда (BCNF):
Удостовериться, что для каждой нетривиальной функциональной зависимости X -> Y, X является кандидатным ключом.

Четвёртая нормальная форма (4NF):
Если есть многозначные зависимости, разделять их в отдельные таблицы.
Например, можно выделить таблицу "Проекты_Сотрудники" для хранения связей между проектами и сотрудниками.
