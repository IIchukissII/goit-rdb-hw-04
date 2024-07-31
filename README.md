
# Домашнє завдання: Управління реляційними базами даних

## 1. Створення бази даних

### Схема бази даних: "LibraryManagement"

#### a. Створення таблиці `authors`

```sql
CREATE TABLE authors (
    author_id INT AUTO_INCREMENT PRIMARY KEY,
    author_name VARCHAR(255) NOT NULL
);
```


#### b. Створення таблиці genres
```sql
CREATE TABLE genres (
    genre_id INT AUTO_INCREMENT PRIMARY KEY,
    genre_name VARCHAR(255) NOT NULL
);
```

#### c. Створення таблиці books
```sql
DROP TABLE IF EXISTS books;

CREATE TABLE books (
    book_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    publication_year YEAR NOT NULL,
    author_id INT,
    genre_id INT,
    FOREIGN KEY (author_id) REFERENCES authors(author_id),
    FOREIGN KEY (genre_id) REFERENCES genres(genre_id)
);
```
#### d. Створення таблиці users
```sql
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL
);
```

#### e. Створення таблиці borrowed_books
```sql
CREATE TABLE borrowed_books (
    borrow_id INT AUTO_INCREMENT PRIMARY KEY,
    book_id INT,
    user_id INT,
    borrow_date DATE NOT NULL,
    return_date DATE,
    FOREIGN KEY (book_id) REFERENCES books(book_id),
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);
```

#### 2. Вставка тестових даних
##### Вставка даних у таблицю authors
```sql
INSERT INTO authors (author_name) VALUES 
('George Orwell'),
('Aldous Huxley'),
('Erich Fromm');
```

###### Вставка даних у таблицю genres
```sql
INSERT INTO genres (genre_name) VALUES 
('Dystopian'),
('Fantasy'),
('Adventure'),
('Romance');
```

##### Вставка даних у таблицю books
```sql
INSERT INTO books (title, publication_year, author_id, genre_id) VALUES 
('1984', 1949, 1, 1),
('Brave New World', 1932, 2, 1),
('Escape from Freedom', 1941, 3, 1);
```

##### Вставка даних у таблицю users
```sql
INSERT INTO users (username, email) VALUES
('JohnDoe', 'john.doe@example.com'),
('JaneSmith', 'jane.smith@example.com'),
('AliceJohnson', 'alice.johnson@example.com'),
('BobBrown', 'bob.brown@example.com'),
('CharlieDavis', 'charlie.davis@example.com');
```

##### Вставка даних у таблицю borrowed_books
```sql
INSERT INTO borrowed_books (book_id, user_id, borrow_date, return_date) VALUES 
(1, 1, '2024-01-15', '2024-02-15'),
(2, 2, '2024-02-01', '2024-02-28'),
(3, 3, '2024-01-20', '2024-03-01'),
(1, 4, '2024-03-05', NULL), -- Ще не повернуто
(2, 5, '2024-03-10', NULL);
```

#### 3. Запит з об'єднанням таблиць
```sql
SELECT 
    od.order_id,
    od.product_id,
    od.quantity,
    o.date AS order_date,
    c.id AS customer_id,
    c.name AS customer_name,
    p.name AS product_name,
    cat.name AS category_name,
    e.first_name AS employee_first_name,
    e.last_name AS employee_last_name,
    s.name AS shipper_name,
    sup.name AS supplier_name
FROM 
    order_details od
INNER JOIN 
    orders o ON od.order_id = o.id
INNER JOIN 
    customers c ON o.customer_id = c.id
INNER JOIN 
    products p ON od.product_id = p.id
INNER JOIN 
    categories cat ON p.category_id = cat.id
INNER JOIN 
    employees e ON o.employee_id = e.employee_id
INNER JOIN 
    shippers s ON o.shipper_id = s.id
INNER JOIN 
    suppliers sup ON p.supplier_id = sup.id;
```
![Результат запиту з об'єднанням таблиць](https://github.com/IIchukissII/goit-rdb-hw-04/blob/main/img/1.PNG)
	
#### 4. Запити
##### 1. Визначення кількості рядків
```sql
SELECT COUNT(*)
FROM order_details od
INNER JOIN orders o ON od.order_id = o.id
INNER JOIN customers c ON o.customer_id = c.id
INNER JOIN products p ON od.product_id = p.id
INNER JOIN categories cat ON p.category_id = cat.id
INNER JOIN employees e ON o.employee_id = e.employee_id
INNER JOIN shippers s ON o.shipper_id = s.id
INNER JOIN suppliers sup ON p.supplier_id = sup.id;
```
![Результат запиту з об'єднанням таблиць](https://github.com/IIchukissII/goit-rdb-hw-04/blob/main/img/2.PNG)

##### 2. Заміна INNER JOIN на LEFT JOIN або RIGHT JOIN
```sql
SELECT COUNT(*)
FROM order_details od
RIGHT JOIN orders o ON od.order_id = o.id
RIGHT JOIN customers c ON o.customer_id = c.id
LEFT JOIN products p ON od.product_id = p.id
LEFT JOIN categories cat ON p.category_id = cat.id
LEFT JOIN employees e ON o.employee_id = e.employee_id
LEFT JOIN shippers s ON o.shipper_id = s.id
LEFT JOIN suppliers sup ON p.supplier_id = sup.id;
```
![Результат запиту з об'єднанням таблиць](https://github.com/IIchukissII/goit-rdb-hw-04/blob/main/img/3.PNG)

```
INNER JOIN повертає лише ті рядки, які мають відповідні значення в обох таблицях.
LEFT JOIN повертає всі рядки з лівої таблиці та відповідні рядки з правої таблиці. Якщо немає відповідного рядка в правій таблиці, то результат міститиме NULL.
RIGHT JOIN працює аналогічно до LEFT JOIN, але повертає всі рядки з правої таблиці.
```

##### 3. Вибір рядків з employee_id > 3 та ≤ 10
```sql
SELECT *
FROM order_details od
INNER JOIN orders o ON od.order_id = o.id
INNER JOIN customers c ON o.customer_id = c.id
INNER JOIN products p ON od.product_id = p.id
INNER JOIN categories cat ON p.category_id = cat.id
INNER JOIN employees e ON o.employee_id = e.employee_id
INNER JOIN shippers s ON o.shipper_id = s.id
INNER JOIN suppliers sup ON p.supplier_id = sup.id
WHERE e.employee_id > 3 AND e.employee_id <= 10;
```
![Результат запиту з об'єднанням таблиць](https://github.com/IIchukissII/goit-rdb-hw-04/blob/main/img/5.PNG)

##### 4. Групування за категорією та підрахунок рядків у групі, середньої кількості товару
```sql
SELECT 
    cat.name AS category_name,
    COUNT(*) AS row_count,
    AVG(od.quantity) AS average_quantity
FROM order_details od
INNER JOIN products p ON od.product_id = p.id
INNER JOIN categories cat ON p.category_id = cat.id
GROUP BY cat.name;
```
![Результат запиту з об'єднанням таблиць](https://github.com/IIchukissII/goit-rdb-hw-04/blob/main/img/6.PNG)

##### 5. Відфільтрувати рядки, де середня кількість товару більша за 21
```sql
SELECT 
    cat.name AS category_name,
    COUNT(*) AS row_count,
    AVG(od.quantity) AS average_quantity
FROM order_details od
INNER JOIN products p ON od.product_id = p.id
INNER JOIN categories cat ON p.category_id = cat.id
GROUP BY cat.name
HAVING AVG(od.quantity) > 21;
```
![Результат запиту з об'єднанням таблиць](https://github.com/IIchukissII/goit-rdb-hw-04/blob/main/img/7.PNG)

##### 6. Сортування рядків за спаданням кількості рядків
```sql
SELECT 
    cat.name AS category_name,
    COUNT(*) AS row_count,
    AVG(od.quantity) AS average_quantity
FROM order_details od
INNER JOIN products p ON od.product_id = p.id
INNER JOIN categories cat ON p.category_id = cat.id
GROUP BY cat.name
HAVING AVG(od.quantity) > 21
ORDER BY row_count DESC;
```
![Результат запиту з об'єднанням таблиць](https://github.com/IIchukissII/goit-rdb-hw-04/blob/main/img/8.PNG)

##### 7. Вивести чотири рядки з пропущеним першим рядком
```sql
SELECT 
    cat.name AS category_name,
    COUNT(*) AS row_count,
    AVG(od.quantity) AS average_quantity
FROM order_details od
INNER JOIN products p ON od.product_id = p.id
INNER JOIN categories cat ON p.category_id = cat.id
GROUP BY cat.name
![Результат запиту з об'єднанням таблиць](https://github.com/IIchukissII/goit-rdb-hw-04/blob/main/img/9.PNG)
HAVING AVG(od.quantity) > 21
ORDER BY row_count DESC
LIMIT 1, 4; -- Пропустити перший рядок і взяти чотири наступні
```
