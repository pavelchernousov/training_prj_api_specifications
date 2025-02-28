🔹 Спецификация API для интеграции CRM ↔ Склад
Метод	URL	Описание	Тело запроса	Ответ	Коды ответа
GET	/products	Получить список товаров на складе	❌	JSON-массив товаров	200 OK, 500 Internal Server Error
GET	/products/{id}	Получить данные о конкретном товаре	❌	JSON-объект товара	200 OK, 404 Not Found
POST	/orders	Создать новый заказ	JSON с customer_id, items, total_price	JSON с order_id и status	201 Created, 400 Bad Request, 500 Internal Server Error
GET	/orders/{id}	Получить информацию о заказе	❌	JSON с деталями заказа	200 OK, 404 Not Found
PATCH	/order-status/{order_id}	Обновить статус заказа	JSON с status	JSON с обновленным статусом	200 OK, 400 Bad Request, 404 Not Found
