# Learning-Database (Relational DBMS focus)
===========================================================================================
## Resource:
- [ ] joins animation: https://dataschool.com/how-to-teach-people-sql/left-right-join-animated/
## One-to-One relationship
- [ ] 1 record in a table is associated with one and only one record in another table
![](https://github.com/hlongn2469/Learning-database-DBMS/blob/main/onetoone.png)
## One-to-Many relationship
- [ ] For example: one customer to many orders
- [ ] Needs **primary** key in customer table and **foreign** key in order table
  ```js
  CREATE TABLE customers(
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(100)
  );
  CREATE TABLE orders(
      id INT AUTO_INCREMENT PRIMARY KEY,
      order_date DATE,
      amount DECIMAL(8,2),
      customer_id INT,
      FOREIGN KEY(customer_id) REFERENCES customers(id)
  );
  ```

## Joins
  ![image](https://user-images.githubusercontent.com/78957509/129669070-e70a2ad7-8fbd-49bd-b892-cb57ddd8da4a.png)

## Cross joins (Outer joins)
- [ ] **Not meaningful**
  ```js
  SELECT * FROM customers, orders; 
  ```
## Inner joins
- [ ] Implicit: 
  ```js
    SELECT * FROM customers, orders 
    WHERE customers.id = orders.customer_id;
  ```
- [ ] Explicit: Use join keyword
  ```js
  SELECT * FROM customers
  JOIN orders
      ON customers.id = orders.customer_id;
  ```
## Left joins
```js
SELECT * FROM customers
LEFT JOIN orders
    ON customers.id = orders.customer_id;
```
## Many-to-Many relationship


