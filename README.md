```markdown
# 🛒 E-Commerce Telegram Bot

Telegram-бот для интернет-магазина, реализованный на **Python (Aiogram 3.x)** с использованием **PostgreSQL**.  
Поддерживает полный цикл покупки: от просмотра каталога до оформления заказа и панели администратора.

---

## 🚀 Возможности

### Для пользователя
- 📂 Каталог товаров
  - Просмотр категорий и товаров
  - Детальная карточка товара (название, описание, цена, фото)
- 🛒 Корзина
  - Добавление/удаление товаров
  - Изменение количества
  - Подсчет общей суммы
- 📦 Заказ
  - Ввод имени, телефона и адреса
  - Выбор способа доставки
  - Подтверждение заказа и генерация номера

### Для администратора
- ➕ Добавление и редактирование товаров
- 📋 Просмотр заказов
- 🔄 Изменение статуса заказов
- 🔔 Уведомления о новых заказах

---

## 📂 Архитектура проекта

```

app/
bot.py                # Точка входа
config.py             # Настройки (токен, БД)
database/
db.py               # Подключение к БД
models.py           # SQLAlchemy модели
handlers/
catalog.py          # Каталог товаров
cart.py             # Корзина
order.py            # Заказы
admin.py            # Админ-панель
keyboards/            # Inline/Reply клавиатуры
services/             # Бизнес-логика
tests/                # Unit-тесты
utils/                # Хелперы, middlewares

````

- **Разделение логики**: бизнес-логика → `services/`, хендлеры → `handlers/`.
- **ORM**: SQLAlchemy + Alembic для миграций.
- **FSM**: для пошагового оформления заказа.
- **Логирование**: Python `logging`.

---

## 🗄 Схема базы данных

```mermaid
erDiagram
    Category ||--o{ Product : contains
    User ||--o{ Cart : owns
    Product ||--o{ Cart : in
    Order ||--o{ OrderItem : includes
    User ||--o{ Order : places

    Category {
        int id PK
        text name
    }
    Product {
        int id PK
        int category_id FK
        text name
        text description
        numeric price
        text photo_url
    }
    User {
        int id PK
        bigint telegram_id
    }
    Cart {
        int id PK
        int user_id FK
        int product_id FK
        int quantity
    }
    Order {
        int id PK
        int user_id FK
        text status
        text address
        text phone
        text name
    }
    OrderItem {
        int id PK
        int order_id FK
        int product_id FK
        int quantity
        numeric price
    }
````

---

## ⚙️ Установка и запуск

### 1. Клонирование репозитория

```bash
git clone https://github.com/USERNAME/ecommerce-bot.git
cd ecommerce-bot
```

### 2. Установка зависимостей

```bash
python -m venv venv
source venv/bin/activate   # или venv\Scripts\activate на Windows
pip install -r requirements.txt
```

### 3. Настройка БД

* Установите PostgreSQL
* Создайте базу данных:

```sql
CREATE DATABASE ecommerce_bot;
```

* Примените миграции (Alembic):

```bash
alembic upgrade head
```

### 4. Настройка переменных окружения

Создайте `.env`:

```
BOT_TOKEN=your_telegram_token
DATABASE_URL=postgresql+asyncpg://user:password@localhost:5432/ecommerce_bot
ADMINS=123456789  # Telegram ID администраторов
```

### 5. Запуск бота

```bash
python app/bot.py
```

---

## 🧪 Тестирование

* Unit-тесты находятся в папке `tests/`.
* Запуск:

```bash
pytest -v
```

Примеры тестов:

* Добавление товара в корзину
* Подсчет суммы заказа
* Обработка ошибок при неверных данных

---

## 📋 Требования к проекту

* Чистый и читаемый код
* Разделение логики по слоям
* Обработка ошибок и исключений
* Логирование
* Документация
* Unit-тесты (3-5 шт.)
* Схема БД
* Инструкция по запуску

---

## 🛠 Технологии

* Python 3.11+
* Aiogram 3.x
* PostgreSQL
* SQLAlchemy + Alembic
* Pytest
* Docker (опционально)

---

## 📜 Лицензия

MIT

```

---


```
