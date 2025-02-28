Учебный проект **Спецификация API**

на примере **Обмен данными между CRM и складской системой**

#### 📌 **Описание задачи**  
Компания использует **CRM** для работы с клиентами и **складскую систему** для учета товаров. Нужно автоматизировать обмен данными, чтобы:  
1. **Обновлять информацию о наличии товаров в CRM** после изменений на складе.  
2. **Передавать заказы из CRM в складскую систему** для их резервирования и отгрузки.  

#### 🎯 **Цели интеграции**  
- Автоматическое обновление данных о товарах.  
- Минимизация ручного ввода данных.  
- Исключение ситуаций, когда менеджеры продают товар, которого нет на складе.
- 
**План работы** 
- *Определяем сущности, участвующие в процессе*
- *Строим диаграмму последовательности с помощью PlantUML*
- *Знакомимся с HTTP и REST-like API*
- *Выявляем ресурсы*
- *Определяем методы*
- *Описание спецификации REST API*
- *Описываем спецификацию/контракт API в табличном виде*
- *Узнаём, чем отличаются идентификация, аутентификация и авторизация*
- *Рассматриваем версионирование API*
- *Обсуждаем нефункциональные требования*
- *Рассматриваем примеры API*
---
### 🔹 **Шаг 1: Разбираем бизнес-процессы, требующие интеграции**  

В нашей задаче участвуют две системы:  
- **CRM** (работают менеджеры, создают заказы).  
- **Складская система** (учет товаров, отгрузка, резервация).  

#### 🔥 **Ключевые бизнес-процессы для интеграции**  
1. **Обновление остатков товаров в CRM**  
   - Менеджеры в CRM должны видеть актуальные остатки.  
   - Когда товар поступает или списывается на складе, информация обновляется.  

2. **Передача заказа со склада в CRM**  
   - Менеджер создает заказ в CRM.  
   - CRM отправляет заказ в складскую систему.  
   - Если товар есть, он резервируется.  
   - Если товара нет, CRM получает уведомление и информирует менеджера.  

3. **Подтверждение отгрузки заказа**  
   - Когда заказ отгружен со склада, CRM получает подтверждение.  
   - Менеджер видит статус ("Отгружено") и может уведомить клиента.  

#### 📌 **Дополнительные нюансы**  
- Ошибки при передаче данных (например, если склад недоступен).  
- Возможность частичной отгрузки заказа.  
- Версионирование API для будущих обновлений.  
---  
### 🔹 **Шаг 2: Определяем сущности, участвующие в процессе**  

При интеграции CRM и складской системы основными **сущностями** (объектами данных) будут:  

1. **Товар (Product)**  
   - `id` — уникальный идентификатор товара.  
   - `name` — название товара.  
   - `sku` — артикул.  
   - `quantity` — количество товара на складе.  
   - `price` — цена.  
   - `updated_at` — дата последнего обновления.  

2. **Заказ (Order)**  
   - `id` — уникальный идентификатор заказа.  
   - `customer_id` — ID клиента в CRM.  
   - `items` — список товаров в заказе (id товара, количество).  
   - `total_price` — общая сумма заказа.  
   - `status` — статус заказа (например, "Новый", "В резерве", "Отгружен").  
   - `created_at` — дата создания.  

3. **Клиент (Customer)** (если CRM передает данные о клиенте)  
   - `id` — уникальный ID клиента.  
   - `name` — имя клиента.  
   - `email` — почта.  
   - `phone` — телефон.  

4. **Уведомление об изменении статуса заказа (OrderStatusUpdate)**  
   - `order_id` — ID заказа.  
   - `status` — новый статус заказа.  
   - `updated_at` — время обновления.  

---

Отлично! Давай разберем основы **HTTP и REST-like API**.  

---

### 🔹 **Что такое REST API?**  
**REST API (Representational State Transfer API)** — это способ общения между клиентом и сервером через HTTP.  

**Пример:**  
Когда ты заходишь в интернет-магазин, твой браузер (клиент) делает запрос к серверу, который возвращает страницу с товарами. REST API работает по такому же принципу, но без интерфейса — данные передаются в формате **JSON** или **XML**.  

---

### 🔹 **Основные HTTP-методы в REST API**  
| **Метод**  | **Описание**                                    | **Пример запроса**               |
|------------|--------------------------------|---------------------------------|
| `GET`      | Получение данных (чтение)      | `GET /products` — получить список товаров |
| `POST`     | Создание нового ресурса       | `POST /orders` — создать новый заказ |
| `PUT`      | Полное обновление ресурса     | `PUT /products/1` — обновить товар с ID=1 |
| `PATCH`    | Частичное обновление ресурса  | `PATCH /orders/5` — изменить статус заказа |
| `DELETE`   | Удаление ресурса              | `DELETE /products/3` — удалить товар с ID=3 |

---

### 🔹 **Пример REST API для интеграции CRM и склада**  
Допустим, CRM хочет получить актуальный список товаров на складе.  

📌 **Запрос:**  
```http
GET /products
Host: warehouse.example.com
```

📌 **Ответ (JSON):**  
```json
[
    {
        "id": 1,
        "name": "Ноутбук",
        "sku": "LAP123",
        "quantity": 15,
        "price": 80000
    },
    {
        "id": 2,
        "name": "Мышь беспроводная",
        "sku": "MOUSE456",
        "quantity": 50,
        "price": 1500
    }
]
```

---

### 🔹 **Коды ответа HTTP**  
| **Код**  | **Описание**                                   | **Пример** |
|----------|----------------------------------|------------------------------|
| `200 OK` | Запрос успешен                  | Товары успешно получены |
| `201 Created` | Объект создан                 | Новый заказ добавлен |
| `400 Bad Request` | Ошибка в запросе             | Отсутствует обязательное поле |
| `401 Unauthorized` | Требуется авторизация       | Нужен API-ключ |
| `404 Not Found` | Ресурс не найден             | Товар с таким ID не найден |
| `500 Internal Server Error` | Ошибка сервера | Проблема в базе данных |

---

### 🔥 **Что дальше?**  
Следующий шаг — **Выявляем ресурсы** (определяем, какие URL-адреса API нужны для интеграции).  

Как думаешь, какие **ресурсы** нам понадобятся для взаимодействия CRM и склада? Или разобрать вместе? 😊
