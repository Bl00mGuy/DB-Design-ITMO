<h2 style="text-align: center;">МИНИСТЕРСТВО ОБРАЗОВАНИЯ И НАУКИ<br/>РОССИЙСКОЙ ФЕДЕРАЦИИ<br/>
ФЕДЕРАЛЬНОЕ ГОСУДАРСТВЕННОЕ АВТОНОМНОЕ ОБРАЗОВАТЕЛЬНОЕ
УЧРЕЖДЕНИЕ ВЫСШЕГО ОБРАЗОВАНИЯ
</h2>

<p style="text-align: center;">«Национальный исследовательский университет ИТМО»</p>

<p style="text-align: center; margin-bottom: 200px">Факультет информационных технологий и программирования</p>

<p style="text-align: center; margin-bottom: 150px">“Проектирование баз данных”</p>

<h4 style="text-align: center;">Лабораторная работа №1.<br/>Вариант №5.</h3>

<p style="margin-left: 400px">Выполнил: Гаджиев Саид Ильясович M3215</p>

<p style="margin-left: 400px; margin-bottom: 250px">Преподаватель: Кошелев Даниил Викторович</p>

<p style="text-align: center; margin-bottom: 50px">Санкт-Петербург<br/>2024</p>

#

<h3>Вариант №5. Яндекс-Маркет.(https://market.yandex.ru)</h3>

<h4>Задачи:</h4>

1. Провести анализ функционала сайта или портала в выбранной предметной области с позиции работы с данными, выделить сущности, их атрибуты и связи между сущностями.

2. Спроектировать архитектуру БД для выбранной темы в виде ER-diagram в нотации IDEF1X или UML.

## Сущности и атрибуты:

1. **Пользователь (User)**:
   - user_id (UUID SERIAL PRIMARY KEY)
   - username (VARCHAR)
   - password (CHKPASS)
   - name (VARCHAR)
   - surname (VARCHAR)
   - phone_number (VARCHAR)
   - address (VARCHAR)
   - plus_subscriber (BOOLEAN)

2. **Заказ (Order)**:
   - order_id (UUID SERIAL PRIMARY KEY)
   - user_id (Foreign Key INT)
   - order_state_id (Foreign Key INT)
   - shop_id (Foreign Key INT)

3. **Магазин (Shop)**:
   - shop_id (UUID SERIAL PRIMARY KEY)
   - name (VARCHAR)
   - address (VARCHAR)
   - email (VARCHAR)
   - phone_number (VARCHAR)
   - rating (NUMERIC)
   - rating_votes (INT)

4. **Единица заказа (OrderUnit)**:
   - order_unit_id (UUID SERIAL PRIMARY KEY)
   - amount (INT)
   - order_id (Foreign Key INT)
   - product_id (Foreign Key INT)

6. **Товар (Product)**:
   - product_id (UUID SERIAL PRIMARY KEY)
   - name (VARCHAR)
   - description (TEXT)
   - price (NUMERIC)
   - rating (NUMERIC)
   - rating_votes (INT)
   - product_category_id (Foreign Key INT)
   - rating_id (Foreign Key INT)

7. **Категория товара (Category)**:
   - category_id (UUID SERIAL PRIMARY KEY)
   - name (TEXT)

8. **Опции товара (ProductOption)**:
    - product_option_id (UUID SERIAL PRIMARY KEY)
    - value (TEXT)
    - product_id (Foreign Key INT)
    - category_option_id (Foreign Key INT)

## Обоснование нахождения модели данных в 3НФ

1НФ: Каждый кортеж отношения содержит только одно значение для каждого из атрибутов.

2НФ: Все атрибуты зависят от потенциального ключа (в модели данных отсутствуют атрибуты, которые не зависят от первичного ключа).

3НФ: Отсутствуют транзитивные функциональные зависимости от потенциального ключа.

![ER diagram](images/Lab-1.svg)

<h4>UML</h4>

```
@startuml
title Яндекс-Маркет

hide circle
skinparam linetype ortho

entity Users {
    user_id UUID SERIAL PRIMARY KEY
    --
    username VARCHAR
    password CHKPASS
    name VARCHAR
    surname VARCHAR
    phone_number VARCHAR
    address VARCHAR
    plus_subscriber BOOLEAN
}

entity Orders {
    order_id UUID SERIAL PRIMARY KEY
    --
    user_id (Foreign Key) INT
    order_unit_id (Foreign Key) INT
    shop_id (Foreign Key) INT
}

entity Shops {
    shop_id UUID SERIAL PRIMARY KEY
    --
    name VARCHAR
    address VARCHAR
    email VARCHAR
    phone_number VARCHAR
    rating NUMERIC
}

entity OrderUnits {
    order_unit_id UUID SERIAL PRIMARY KEY
    --
    amount INT
    --
    order_id (Foreign Key) INT
    product_id (Foreign Key) INT
}

entity Products {
    product_id UUID SERIAL PRIMARY KEY
    --
    name VARCHAR
    description TEXT
    price NUMERIC
    rating NUMERIC
    rating_votes INT
    --
    category_id (Foreign Key) INT
    product_option_id (Foreign Key) INT
}

entity Categories {
    category_id UUID SERIAL PRIMARY KEY
    --
    name TEXT
}

entity ProductOptions {
    product_option_id UUID SERIAL PRIMARY KEY
    --
    value TEXT
    --
    product_id (Foreign Key) INT
}

Products }o-right-|| Categories
ProductOptions }o-up-o{ Products
Shops ||--o{ Products
Orders }o--|| Shops
Orders }o-up-|| Users
OrderUnits }|-up-|| Orders
OrderUnits }o--|| Products

@enduml
```
