






## Relational Database

### Database (資料庫)
有組織的資料集合，儲存在硬碟中。

一個 Database 中包含很多個 table (資料表)。

常見的資料庫系統: `MySQL`, `SQLite`, `Oracle`, `NoSQL` 等


### Table (資料表)
資料的儲存單位，類似 excel 表格。

每一張 Table 會有 column (欄) 和 row (列):
- **Column (欄)** : 欄位名稱、型別 (ex: `name: VARCHAR(50)`)
- **Row (列)** : 實際的每筆資料 (ex: `('Hane')` )

### Primary Key (主鍵)

- 可以唯一識別一筆資料的欄位
- 不可以重複也不可以是空值 ( null )

通常是唯一的`id`

一個 table 中只能有一個 Primary Key ， 可以是單欄或複合欄位。

### Foreign Key (外鍵)

- 指向其他 table 的 Primary Key，用來建立 **table 之間的關係**
- 可以一對一、一對多、多對一

### Constraints (約束)

- 用來限制欄位的有效性: 

| Constraint    | 說明                      |
| ------------- | ----------------------- |
| `PRIMARY KEY` | 唯一、非空                   |
| `FOREIGN KEY` | 外部關聯                    |
| `UNIQUE`      | 不可重複                    |
| `NOT NULL`    | 不可為空                    |
| `CHECK`       | 資料條件（e.g. `age >= 0`） |
| `DEFAULT`     | 預設值                     |

### Schema (資料庫結構設計)
- 整個 database 中 table 的設計藍圖 (欄位、關聯、型別、主鍵、外鍵等)
- `CREATE TABLE`、`ER diagram`

### Relational Database

關聯式指的是 table 之間用 Primary / Foreign Key 連接

ex: 學生、課程、成績等

### 型別範例:

| 型別           | 用途 | 範例                  |
| ------------ | -- | ------------------- |
| `INT`        | 整數 | `age INT`           |
| `VARCHAR(n)` | 字串 | `name VARCHAR(50)`  |
| `DATE`       | 日期 | `birthday DATE`     |
| `BOOLEAN`    | 布林 | `is_active BOOLEAN` |


---

## ER Diagram (實體關聯圖)

ER: Entity Relationship

- Entity (實體)
- Attribute (屬性)
- Realationship (關聯)

主要目的是: 在寫 SQL 前，清楚定義資料的邏輯結構與關聯。

| 名稱                   | 符號     | 意思                     |
| -------------------- | ------ | ---------------------- |
| Entity（實體）      | 長方形    | 真實世界中的對象 |
| Attribute（屬性）   | 橢圓形    | 描述實體的欄位   |
| Primary Key      | 橢圓形＋底線 | 唯一識別的屬性    |
| Relationship（關聯） | 菱形     | 兩個實體之間的互動   |
| Cardinality（基數）  | 線上標註   | 一對一、一對多、多對多的關係   |


ex:

```pgsql
[Student] ------<Enrolls>------ [Course]
   |                             |
 (student_id)                (course_id)
    name                         name
    age                          credit

```
- `Student`、`Course` 是 Entity
- `Enrolls` 是 Relationship
- `student_id` 和 `course_id` 是 Primary Key
- `Enrolls` 是一個多對多 (M:N) 的關係


### ER Diagram 轉 Relational Database

ER diagram 只是概念圖，最後還是要轉成 SQL 的資料表

- Entity => 一張 Table
- Attribute => 欄位 column
- Primary Key => 主鍵欄位
- 1:N => 用 foreign key 連接
- M:N => 多用一個表儲存兩邊的 Primary Key 做對應



---

## SQL 語法

### 基本查詢、條件

#### SELECT / FROM / WHERE

```sql
SELECT column1, column2 //要的欄位
FROM table_name;  //從哪些 table 拿
WHERE condition  //條件
```

ex:
```sql
SELECT name, age
FROM students;
```
拿 `students` table 裡面的 `name` 和 `age` 欄位


#### WHERE 中常用的比較運算子

- `>`, `<`, `=`, `>=`, `<=`: 數值比較
- `!=` `<>`: 這兩個都是不等於
- `BETWEEN a AND b`: 介於 `a` 和 `b` 之間
- `LIKE`: 模糊比對 用在比較字串 (`%`表示可以是任意字串、`_`表示任意字元) ex: `WHERE Products Like '_e%'`，表示第二個字元是`e`的字串
- `IN(Value1, Value2, ...)`: 符合指定的其中一個值

#### WHERE 中的邏輯運算子

基本上就是 `AND` `OR` `NOT`，也可以有多層，用`()`決定先後順序。

ex:
```sql
SELECT name
FROM employees
WHERE (department = 'IT' OR department = 'HR') AND salary > 55000;
```
加`()`確保了先做`OR`再做`AND`

#### 其他

`IS NULL`、`IS NOT NULL`: 找沒有/有填欄位值的資料

ex:
```sql
SELECT name
FROM employees
WHERE salary IS NULL;
```
找沒有填薪資的員工


#### ORDER BY
用某個欄位值來排序 預設是`ASC`
- ASC: 由小到大
- DESC: 由大到小
```sql
SELECT * FROM students
ORDER BY age ASC;
```

#### LIMIT
顯示前 n 筆資料

ex:
```sql
SELECT * FROM students
ORDER BY age DESC 
LIMIT 5;
```
照 age 從大到小排 只顯示前五筆資料

#### DISTINCT
放在 SELECT 後面，用來去掉重複的資料
```sql
SELECT DISTINCT department FROM students;
```

---

### 資料操作

資料表中的新增(`INSERT`)、修改(`UPDATE`)、刪除(`DELETE`)，這些語法會用在 API 裡面。

1) `INSERT INTO`: 新增資料
```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```
ex:
```sql
INSERT INTO employees (id, name, age, department, salary)
VALUES (6, 'Fiona', 29, 'HR', 55000);
```

可以一次輸入多筆資料:
```sql
INSERT INTO employees (id, name, age, department, salary)
VALUES 
(7, 'Grace', 26, 'IT', 52000),
(8, 'Henry', 33, 'IT', 60000);
```



2) `UPDATE`: 修改資料
```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```
ex:
```sql
UPDATE employee
SET salray = 70000
WHERE name="Bob";
```

3) `DELETE`: 刪除資料
```sql
DELETE FROM employee
WHERE department = "Sales";
```

`UPDATE`和`DELETE`通常都要配`WHERE`，不然整個資料表都會被改到

### JOIN (資料表關聯查詢)

Relational Database 中資料會拆分成多個 table ，要取得完整資訊就需要把多個不同 table 連接起來 ex:`user`和`orders`

`JOIN`分為
- `INNER JOIN`: 只選兩表中有關聯的資料
- `LEFT JOIN`: 保留左表的全部，右表沒有的補 NULL
- `RIGHT JOIN`: 保留右表全部，左表沒有的補 NULL
- `FULL JOIN`: 左右表全部保留，沒對應的補 NULL

( FROM 後面的會是左表 )

語法:
```sql
SELECT A.column1, B.column2
FROM A
INNER JOIN B ON A.key = B.key;
```


#### ex: 有兩個資料表`users`和`orders`，兩者的關聯在id

**users**
| id | name    |
| -- | ------- |
| 1  | Alice   |
| 2  | Bob     |
| 3  | Charlie |

**orders**
| id  | user_id | product |
| --- | -------- | ------- |
| 101 | 1        | Book    |
| 102 | 2        | Laptop  |
| 103 | 2        | Mouse   |
| 104 | 4        | Pen     |

如果用`INNER JOIN`:

```sql
SELECT users.name, orders.product
FROM orders
INNER JOIN users ON orders.user_id = users.id;
```
這樣兩邊都有對應到的key的資料才會保留

| name  | product |
| ----- | ------- |
| Alice | Book    |
| Bob   | Laptop  |
| Bob   | Mouse   |

如果用`LEFT JOIN`:

```sql
SELECT users.name, orders.product
FROM orders
LEFT JOIN users ON orders.user_id = users.id
```
這樣就會以 orders 為主，就算沒有對應到user也會保留填null進去
| name  | product |
| ----- | ------- |
| Alice | Book    |
| Bob   | Laptop  |
| Bob   | Mouse   |
| NULL  | Pen     |


---

### GROUP BY 和 Aggregate Function

`GROUP BY`: 根據某個欄位把資料分組，再對每組資料做統計、計算

```sql
SELECT group_column, AGG_FUNC(column)
FROM table
GROUP BY group_column;
```
常見的 Aggregate Functions

| 函數        | 功能   | 範例               |
| --------- | ---- | ---------------- |
| `COUNT()` | 計算筆數 | `COUNT(*)` //總筆數 |
| `SUM()`   | 加總   | `SUM(salary)`    |
| `AVG()`   | 平均值  | `AVG(score)`     |
| `MAX()`   | 最大值  | `MAX(age)`       |
| `MIN()`   | 最小值  | `MIN(age)`       |

ex:

`employees`
| id | name    | department | salary |
| -- | ------- | ---------- | ------ |
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |
| 4  | Diana   | Sales      | 40000  |
| 5  | Ethan   | IT         | 80000  |

查每個部門的平均薪資

```sql
SELECT department, AVG(salary) AS avg_salary 
//`AS` 是alias(別名)的用法
FROM employees
GROUP BY department;
```

查每個部份的員工數



```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```
- 如果有搭配`WHERE`會是先`WHERE`過濾資料以後再分組

ex:
```sql
SELECT department, AVG(salary)
FROM employees
WHERE salary > 50000
GROUP BY department;
```
會先計算 salary>50000 的人再分組


#### HAVING
這是用在 GROUP BY 後面的結果

ex:
```sql
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 60000;
```


---




