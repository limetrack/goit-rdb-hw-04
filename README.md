# DML та DDL команди. Складні SQL вирази

## 1. Створіть базу даних для керування бібліотекою книг згідно зі структурою, наведеною нижче.
a)
```
CREATE SCHEMA IF NOT EXISTS LibraryManagement;
USE LibraryManagement;
```

b)
```
CREATE TABLE authors (
    author_id INT AUTO_INCREMENT PRIMARY KEY,
    author_name VARCHAR(255) NOT NULL
);
```

c)
```
CREATE TABLE genres (
    genre_id INT AUTO_INCREMENT PRIMARY KEY,
    genre_name VARCHAR(255) NOT NULL
);
```

d)
```
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

e)
```
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL
);
```

f)
```
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

<img width="762" alt="p-1-2_1" src="https://github.com/user-attachments/assets/4c14328e-fd01-4346-ba6b-c9b270d0cb63">
<img width="949" alt="p-1-2_2" src="https://github.com/user-attachments/assets/5215650e-3784-4d86-9072-4119efd2fe33">

## 2. Заповніть таблиці простими видуманими тестовими даними. Достатньо одного-двох рядків у кожну таблицю.
```
INSERT INTO authors (author_name) VALUES
('T.G. Shevchenko'),
('G. Orwell');

INSERT INTO genres (genre_name) VALUES
(‘Folk’),
('Dystopian');

INSERT INTO books (title, publication_year, author_id, genre_id) VALUES
(‘Kateryna’, 1863, 1, 1),
('1984', 1949, 2, 2);

INSERT INTO users (username, email) VALUES
(kate_test, kate_test@example.com'),
(petro_test, petro_test@example.com');

INSERT INTO borrowed_books (book_id, user_id, borrow_date, return_date) VALUES
(1, 1, '2024-11-01', '2024-11-10'),
(2, 2, '2024-11-05', NULL);
```

## 3. Напишіть запит за допомогою операторів FROM та INNER JOIN, що об’єднує всі таблиці даних, які ми завантажили з файлів: order_details, orders, customers, products, categories, employees, shippers, suppliers.
```
USE mydb;

SELECT *
FROM 
	   order_details AS od
INNER JOIN 
    orders AS o ON od.order_id = o.id
INNER JOIN                                            
    products AS p ON od.product_id = p.id
INNER JOIN 
    customers AS c ON o.customer_id = c.id
INNER JOIN 
    employees AS e ON o.employee_id = e.employee_id
INNER JOIN 
    shippers AS ship ON o.shipper_id = ship.id
INNER JOIN 
    categories AS cat ON p.category_id = cat.id
INNER JOIN 
    suppliers AS sup ON p.supplier_id = sup.id;
```
    
<img width="1233" alt="p3" src="https://github.com/user-attachments/assets/db619fb2-56b8-4884-a698-d437ec6c1554">

## 4. Виконайте запити, перелічені нижче.
### a) Визначте, скільки рядків ви отримали (за допомогою оператора COUNT).
```
SELECT COUNT(*) AS total_rows
FROM 
	order_details AS od
INNER JOIN 
    orders AS o ON od.order_id = o.id
INNER JOIN                                            
    products AS p ON od.product_id = p.id
INNER JOIN 
    customers AS c ON o.customer_id = c.id
INNER JOIN 
    employees AS e ON o.employee_id = e.employee_id
INNER JOIN 
    shippers AS ship ON o.shipper_id = ship.id
INNER JOIN 
    categories AS cat ON p.category_id = cat.id
INNER JOIN 
    suppliers AS sup ON p.supplier_id = sup.id;
```

<img width="1012" alt="p4_1" src="https://github.com/user-attachments/assets/37c18512-3415-44a3-99ee-60df7f627d7a">

### b) Змініть декілька операторів INNER на LEFT чи RIGHT. Визначте, що відбувається з кількістю рядків. Чому?
Зміна операторів INNER JOIN на LEFT JOIN або RIGHT JOIN впливає на кількість рядків, які повертає запит, оскільки ці типи з'єднань обробляють дані по-різному:
- Використання LEFT JOIN зазвичай збільшує кількість рядків у порівнянні з INNER JOIN, оскільки додаються всі рядки з лівої таблиці, навіть якщо немає відповідності в правій таблиці.
- Аналогічно, RIGHT JOIN збільшує кількість рядків, додаючи всі рядки з правої таблиці, навіть якщо немає відповідності в лівій таблиці.

### c) Оберіть тільки ті рядки, де employee_id > 3 та ≤ 10
```
SELECT COUNT(*) AS total_rows
FROM order_details AS od
INNER JOIN 
    orders AS o ON od.order_id = o.id
INNER JOIN                                            
    products AS p ON od.product_id = p.id
INNER JOIN 
    customers AS c ON o.customer_id = c.id
INNER JOIN 
    employees AS e ON o.employee_id = e.employee_id
INNER JOIN 
    shippers AS ship ON o.shipper_id = ship.id
INNER JOIN 
    categories AS cat ON p.category_id = cat.id
INNER JOIN 
    suppliers AS sup ON p.supplier_id = sup.id
WHERE e.employee_id > 3 AND e.employee_id <= 10;
```

<img width="1010" alt="p4_3" src="https://github.com/user-attachments/assets/f3995eca-e153-4342-9f90-bf7bd44fab36">

### d) Згрупуйте за іменем категорії, порахуйте кількість рядків у групі, середню кількість товару (кількість товару знаходиться в order_details.quantity)
```
SELECT
	   cat.name,
    COUNT(*) AS total_rows,
    AVG(od.quantity) AS average_quantity
FROM order_details AS od
INNER JOIN 
    orders AS o ON od.order_id = o.id
INNER JOIN                                            
    products AS p ON od.product_id = p.id
INNER JOIN 
    categories AS cat ON p.category_id = cat.id
GROUP BY 
    cat.name;
```

<img width="570" alt="p4_4" src="https://github.com/user-attachments/assets/4eab1cad-5705-4966-ba11-96ebbde00ddf">


### e) Відфільтруйте рядки, де середня кількість товару більша за 21.
```
SELECT
	   cat.name,
    COUNT(*) AS total_rows,
    AVG(od.quantity) AS average_quantity
FROM order_details AS od
INNER JOIN 
    orders AS o ON od.order_id = o.id
INNER JOIN                                            
    products AS p ON od.product_id = p.id
INNER JOIN 
    categories AS cat ON p.category_id = cat.id
GROUP BY 
    cat.name
HAVING 
    AVG(od.quantity) > 21;
```

<img width="516" alt="p4_5" src="https://github.com/user-attachments/assets/bcfe09bd-b535-486f-873e-94d0a934ea6c">


### f) Відсортуйте рядки за спаданням кількості рядків.
```
SELECT
	   cat.name,
    COUNT(*) AS total_rows,
    AVG(od.quantity) AS average_quantity
FROM order_details AS od
INNER JOIN 
    orders AS o ON od.order_id = o.id
INNER JOIN                                            
    products AS p ON od.product_id = p.id
INNER JOIN 
    categories AS cat ON p.category_id = cat.id
GROUP BY 
    cat.name
HAVING 
    AVG(od.quantity) > 21
ORDER BY 
    total_rows DESC;
```

<img width="559" alt="p4_6" src="https://github.com/user-attachments/assets/4817c96d-ddc5-4906-8c6a-1ec6e85dd48b">


### g) Виведіть на екран (оберіть) чотири рядки з пропущеним першим рядком.
```
SELECT
	   cat.name,
    COUNT(*) AS total_rows,
    AVG(od.quantity) AS average_quantity
FROM order_details AS od
INNER JOIN 
    orders AS o ON od.order_id = o.id
INNER JOIN                                            
    products AS p ON od.product_id = p.id
INNER JOIN 
    categories AS cat ON p.category_id = cat.id
GROUP BY 
    cat.name
HAVING 
    AVG(od.quantity) > 21
ORDER BY 
    total_rows DESC
LIMIT 4 OFFSET 1;
```

<img width="586" alt="p4_7" src="https://github.com/user-attachments/assets/ea20ad81-34ea-4904-be5f-9cb0a3e8b1c1">





