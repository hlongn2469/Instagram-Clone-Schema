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
Examples:
  - [ ] LEFT join: How many friends and connections do my Facebook friends have? (Regardless of if they are on LinkedIn)
  - [ ] RIGHT join: How many friends and connections do my LinkedIn connections have? (Regardless of if they are on facebook)
  - [ ] INNER join: How many friends and connections do my friends who are on both on Facebook and LinkedIn have?

## Cross joins (Outer joins)
- [ ] **Not meaningful**
  ```js
  SELECT * FROM facebook, linkedin; 
  ```
## Inner joins
- [ ] Implicit: 
  ```js
    SELECT * FROM facebook, linkedin
    WHERE facebook.name = linkedin.facebook_name;
  ```
- [ ] Explicit: Use join keyword
  ```js
  SELECT * FROM facebook
  JOIN linkedin
      ON facebook.name = linkedin.facebook_name;
  ```
## Left joins
```js
  SELECT * FROM facebook
  LEFT JOIN linkedin
      ON facebook.name = linkedin.facebook_name;
```

## Right joins
```js
  SELECT * FROM facebook
  RIGHT JOIN linkedin
      ON facebook.name = linkedin.facebook_name;
```
## Many-to-Many relationship


