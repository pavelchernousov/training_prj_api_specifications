## **Таблица форматов (OpenAPI `format`)**
✔ **В OpenAPI есть 6 основных типов**: `string`, `number`, `integer`, `boolean`, `array`, `object`  
✔ **Можно указывать форматы** (`date`, `uuid`, `email` и т. д.)  
✔ **Поддерживаются массивы и сложные структуры (объекты)**  

|**Тип** | **Формат** | **Комментарий** | **Пример** |
|-----------|--------|-------------|--------|
| integer | int32 | Целое 32-битное число со знаком | `100`, `-5`  |
| integer | int64 | Целое 64-битное число со знаком |  - |
| number | float | Вещественное с плавающей точкой | `12.34`, `0.99` |
| number | double | Вещественное с двойной точностью |  - |
| string | text | Строка | `"text"`, `"hello"` |
| string | password | Скрывать ввод в UI | - |
| string | date | Дата (`YYYY-MM-DD`) |  - |
| string | date-time | Дата + время (`YYYY-MM-DDTHH:MM:SSZ`) | - |
| string | email | Электронная почта | - |
| string | uuid | Уникальный идентификатор | - |
| boolean | | Логический тип (`true` или `false`) | `true`, `false` |
| array | | Массив значений одного типа | `[1, 2, 3]`, `["a", "b", "c"]` |
| object | | Объект (сложная структура с полями) | `{"id": 1, "name": "Product"}` |

---

## **🔹 2. Примеры использования типов в OpenAPI**

### ✅ **Пример `string`**
```yaml
type: string
example: "Ипотека"
```
**Дополнительные свойства для `string`:**  
- `format: date` → Дата в формате `YYYY-MM-DD`
- `format: date-time` → Дата + время (`YYYY-MM-DDTHH:MM:SSZ`)
- `format: email` → Email-адрес  
- `format: uuid` → Уникальный идентификатор  

```yaml
type: string
format: date-time
example: "2025-03-01T12:00:00Z"
```

---

### ✅ **Пример `number` и `integer`**
```yaml
type: number
example: 99.99
```
```yaml
type: integer
example: 10
```
**Дополнительные свойства:**
```yaml
type: integer
minimum: 1
maximum: 100
example: 50
```
*(Ограничивает число от 1 до 100)*

---

### ✅ **Пример `boolean`**
```yaml
type: boolean
example: true
```

---

### ✅ **Пример `array`**
```yaml
type: array
items:
  type: string
example: ["Автокредит", "Ипотека", "Потребительский"]
```

---

### ✅ **Пример `object`**
```yaml
type: object
properties:
  id:
    type: integer
  name:
    type: string
  isActive:
    type: boolean
example:
  id: 1
  name: "Ипотека"
  isActive: true
```

---

## **🔹 3. Комплексные структуры**
**Пример: объект с массивами и вложенными объектами**
```yaml
type: object
properties:
  id:
    type: integer
  name:
    type: string
  conditions:
    type: array
    items:
      type: object
      properties:
        interestRate:
          type: number
        durationMonths:
          type: integer
example:
  id: 1
  name: "Автокредит"
  conditions:
    - interestRate: 5.5
      durationMonths: 60
    - interestRate: 6.2
      durationMonths: 84
```

---

## **🔹 4. Применение типов данных в API**
### 📌 **Пример OpenAPI-документации для `POST`-запроса**
```yaml
post:
  summary: "Создание заявки на кредит"
  requestBody:
    required: true
    content:
      application/json:
        schema:
          type: object
          properties:
            customerId:
              type: integer
            loanType:
              type: string
            amount:
              type: number
            durationMonths:
              type: integer
          required:
            - customerId
            - loanType
            - amount
            - durationMonths
  responses:
    "201":
      description: "Заявка успешно создана"
```

---


