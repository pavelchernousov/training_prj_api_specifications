### 🔹 **Спецификация API для интеграции CRM ↔ Склад**
| **Метод**  | **URL**  | **Описание**  | **Тело запроса**  | **Ответ**  | **Коды ответа** |
|------------|----------|--------------|------------------|------------|----------------|
| **GET** | `/products` | Получить список товаров на складе | ❌ | JSON-массив товаров | `200 OK`, `500 Internal Server Error` |
| **GET** | `/products/{id}` | Получить данные о конкретном товаре | ❌ | JSON-объект товара | `200 OK`, `404 Not Found` |
| **POST** | `/orders` | Создать новый заказ | JSON с `customer_id`, `items`, `total_price` | JSON с `order_id` и `status` | `201 Created`, `400 Bad Request`, `500 Internal Server Error` |
| **GET** | `/orders/{id}` | Получить информацию о заказе | ❌ | JSON с деталями заказа | `200 OK`, `404 Not Found` |
| **PATCH** | `/order-status/{order_id}` | Обновить статус заказа | JSON с `status` | JSON с обновленным статусом | `200 OK`, `400 Bad Request`, `404 Not Found` |


Вот пример **спецификации API** для **GET-запроса на просмотр доступных предложений по кредитам**.

---

### **📊 Спецификация API (GET-запрос: список доступных кредитных предложений)**

| **Параметр запроса**  | **Описание**                                           | **Тип данных** | **Обязательный** | **Пример**            |
|----------------------|--------------------------------------------------------|--------------|----------------|----------------------|
| `loanType`          | Тип кредита (ипотека, авто, потребительский и т. д.)   | `string`      | Нет            | `mortgage`           |
| `minAmount`         | Минимальная сумма кредита                              | `float`       | Нет            | `100000`             |
| `maxAmount`         | Максимальная сумма кредита                             | `float`       | Нет            | `5000000`            |
| `minInterestRate`   | Минимальная процентная ставка                          | `float`       | Нет            | `5.0`                |
| `maxInterestRate`   | Максимальная процентная ставка                         | `float`       | Нет            | `15.0`               |
| `durationMonths`    | Срок кредита в месяцах                                 | `integer`     | Нет            | `240`                |
| `collateral`        | Требуется ли залог (`true/false`)                      | `boolean`     | Нет            | `true`               |
| `guarantorRequired` | Требуется ли поручитель (`true/false`)                 | `boolean`     | Нет            | `false`              |

---

### **📍 Пример запроса**
```http
GET /loans?loanType=mortgage&minAmount=100000&maxInterestRate=15.0 HTTP/1.1
Host: bank.com
Content-Type: application/json
```

---

### **📍 Пример успешного ответа (200 OK)**
```json
{
  "status": "success",
  "offers": [
    {
      "loanId": 1,
      "loanType": "mortgage",
      "amountRange": {
        "min": 100000,
        "max": 5000000
      },
      "interestRate": 7.2,
      "durationMonths": 240,
      "collateralRequired": true,
      "guarantorRequired": false
    },
    {
      "loanId": 2,
      "loanType": "mortgage",
      "amountRange": {
        "min": 300000,
        "max": 7000000
      },
      "interestRate": 6.5,
      "durationMonths": 360,
      "collateralRequired": true,
      "guarantorRequired": true
    }
  ]
}
```

---

### **📍 Пример ошибки (400 Bad Request)**
```json
{
  "status": "error",
  "message": "Invalid interest rate range. The minimum rate cannot be higher than the maximum rate."
}
```

---

### **📍 Спецификация запроса**

| **Параметр**       | **Описание**                                         |
|--------------------|------------------------------------------------------|
| **Метод**          | `GET`                                                |
| **URL**            | `/loans`                                             |
| **Заголовки**      | `Content-Type: application/json`                     |
| **Ответ**          | `200 OK` (Список доступных предложений)              |
| **Коды ошибок**    | `400 Bad Request` (Некорректные параметры запроса)   |

---

### 🔥 **Резюме**
✔ **GET-запрос** позволяет **просматривать доступные кредитные предложения**  
✔ Можно **фильтровать кредиты** по сумме, ставке, сроку, наличию залога и поручителя  
✔ В ответе приходит **список кредитных продуктов** с параметрами  

Если нужно что-то уточнить или доработать – говори! 🚀
