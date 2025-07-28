# MySQL
Работы из университета. Создание таблиц и заполнение их.
Пример одной из работ представлен ниже.


<img width="479" height="361" alt="image" class="center" src="https://github.com/user-attachments/assets/403f1cfc-105f-4f07-bed5-e8c2e722c888" />

Схема ER


SQL-скрипт реализации БД  Библиотека:


CREATE TABLE Books (
book_id INT PRIMARY KEY,
title VARCHAR(100),
author VARCHAR(100),
genre VARCHAR(50)
);

INSERT INTO Books (book_id, title, author, genre) VALUES
(1, 'Война и мир', 'Лев Толстой', 'Роман'),
(2, 'Отцы и дети', 'Иван Тургенев', 'Роман'),
(3, 'Бойцовский клуб', 'Чак Паланик', 'Роман');


CREATE TABLE Readers (
reader_id INT PRIMARY KEY,
name VARCHAR(50),
email VARCHAR(50)
);


INSERT INTO Readers (reader_id, name, email) VALUES
(1, 'Амина', 'amina@gmail.com'),
(2, 'Алина', 'alina@gmail.com');


CREATE TABLE IssBooks (
issue_id INT PRIMARY KEY,
book_id INT,
reader_id INT,
issue_date DATE,
due_date DATE,
FOREIGN KEY (book_id) REFERENCES Books(book_id),
FOREIGN KEY (reader_id) REFERENCES Readers(reader_id)
);


INSERT INTO IssBooks (issue_id, book_id, reader_id, issue_date, due_date) VALUES
(1, 1, 1, '2022-01-01', '2022-02-01'),
(2, 2, 2, '2022-01-15', '2022-02-15');


CREATE TABLE RetBooks (
return_id INT PRIMARY KEY,
issue_id INT,
return_date DATE,
FOREIGN KEY (issue_id) REFERENCES IssBooks(issue_id)
);


INSERT INTO RetBooks (return_id, issue_id, return_date) VALUES
(1, 1, '2022-01-25');

SELECT b.title, r.name
FROM IssBooks ib
JOIN Books b ON ib.book_id = b.book_id
JOIN Readers r ON ib.reader_id = r.reader_id;


CREATE VIEW IssuedBooks AS
SELECT b.title, r.name
FROM IssBooks ib
JOIN Books b ON ib.book_id = b.book_id
JOIN Readers r ON ib.reader_id = r.reader_id
LEFT JOIN RetBooks rb ON ib.issue_id = rb.issue_id
WHERE rb.return_id IS NULL;

