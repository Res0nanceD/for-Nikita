# for-Nikita

### Структура таблиц

#### Таблица Services:
| service_id | service_name  |
|------------|----------------|
| 1          | Cleaning       |
| 2          | Maintenance    |

#### Таблица Components:
| component_id | component_name | price  |
|--------------|----------------|--------|
| 1            | Soap           | 10.00  |
| 2            | Brush          | 15.00  |
| 3            | Detergent      | 8.00   |
| 4            | Screwdriver    | 12.00  |
| 5            | Cleaner        | 5.00   |

#### Таблица ServiceComponents:
| service_id | component_id | quantity | unit   |
|------------|--------------|----------|--------|
| 1          | 1            | 2        | liters |
| 1          | 2            | 1        | piece  |
| 2          | 3            | 5        | kg     |
| 2          | 4            | 2        | piece  |
| 1          | 5            | 1        | bottle |

### SQL-код для создания таблиц

```sql
CREATE TABLE Services (
    service_id AUTOINCREMENT PRIMARY KEY,
    service_name VARCHAR(100) NOT NULL
);

CREATE TABLE Components (
    component_id AUTOINCREMENT PRIMARY KEY,
    component_name VARCHAR(100) NOT NULL,
    price DECIMAL(10, 2) NOT NULL
);

CREATE TABLE ServiceComponents (
    service_id INT,
    component_id INT,
    quantity DECIMAL(10, 2),
    unit VARCHAR(50),
    PRIMARY KEY (service_id, component_id),
    FOREIGN KEY (service_id) REFERENCES Services(service_id),
    FOREIGN KEY (component_id) REFERENCES Components(component_id)
);
```

### Примеры добавления данных

#### Добавление новых данных в таблицы Services и Components:

```sql
INSERT INTO Services (service_name) VALUES 
('Cleaning'), 
('Maintenance');

INSERT INTO Components (component_name, price) VALUES 
('Soap', 10.00), 
('Brush', 15.00), 
('Detergent', 8.00), 
('Screwdriver', 12.00),
('Cleaner', 5.00);
```

#### Добавление новых данных в таблицу ServiceComponents:

```sql
INSERT INTO ServiceComponents (service_id, component_id, quantity, unit) VALUES 
(1, 1, 2, 'liters'),
(1, 2, 1, 'piece'),
(2, 3, 5, 'kg'),
(2, 4, 2, 'piece'),
(1, 5, 1, 'bottle');
```

### Пример изменения данных

#### Изменение количества компонента в услуге:

Допустим, мы хотим изменить количество `Soap` (component_id = 1) в услуге `Cleaning` (service_id = 1) на 3 литра вместо 2.

```sql
UPDATE ServiceComponents
SET quantity = 3
WHERE service_id = 1 AND component_id = 1;
```

### Пример расчета стоимости услуги

#### Расчет стоимости услуги с service_id = 1 (Cleaning):

```sql
SELECT 
    s.service_name,
    SUM(c.price * sc.quantity) AS total_cost
FROM 
    Services s
JOIN 
    ServiceComponents sc ON s.service_id = sc.service_id
JOIN 
    Components c ON sc.component_id = c.component_id
WHERE 
    s.service_id = 1
GROUP BY 
    s.service_name;
```

### Пояснения:

1. **Создание таблиц**: SQL-код создает три таблицы: `Services`, `Components` и `ServiceComponents`. В таблицах используются внешние ключи для поддержания целостности данных.
2. **Добавление данных**: Примеры SQL-кода добавляют записи в таблицы `Services`, `Components` и `ServiceComponents`.
3. **Изменение данных**: Пример SQL-кода показывает, как изменить количество компонента в услуге.
4. **Расчет стоимости**: SQL-запрос вычисляет общую стоимость услуги путем суммирования стоимости компонентов, умноженной на их количество.

### Заключение

Эти примеры показывают, как использовать Microsoft Access для создания и управления базами данных с услугами и их составами, а также для выполнения расчетов стоимости услуг и внесения изменений в данные. Access обеспечивает удобство и гибкость для управления небольшими и средними проектами, предоставляя мощные инструменты для работы с реляционными данными.
