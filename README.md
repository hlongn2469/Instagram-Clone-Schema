# Learning-Database (Relational DBMS focus)
===========================================================================================
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
## Cross joins
- [ ] Used to generate a paired combination of each row of the first table with each row of the second table.
- [ ] *Not meaningful*
  ```js
  SELECT * FROM customers, orders; 
  ```
## Many-to-Many relationship


