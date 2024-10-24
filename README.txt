# Expense Manager App

This application allows you to manage group expenses for trips. It enables adding expenses, distributing the amounts among participants, and making transfers. The data is stored in a MariaDB database and can be reloaded to continue modifying at any time.

## Requirements

- JDK 8 or later
- MariaDB (installed locally or on a remote server)
- MariaDB JDBC Connector (lib/mariadb-java-client.jar)

## Setup

1. Download the `mariadb-java-client.jar` file and place it in the `lib/` folder.
2. Ensure MariaDB is running and create the database by running the SQL script located in the `sql/` folder:

   ```sql
   CREATE DATABASE expense_manager;
   USE expense_manager;
   -- Rest of the SQL commands are in create_database.sql

Edit the DatabaseConnection.java file to provide the correct credentials for your MariaDB database (username and password):

private static final String USER = "root";  // Replace with your username
private static final String PASSWORD = "password";  // Replace with your password

4 Compile and run the application starting from App.java.

Compilation

From the root of the project:

javac -cp "lib/mariadb-java-client.jar" src/*.java -d bin/

Execution

java -cp "bin/:lib/mariadb-java-client.jar" App

Folder Structure
ExpenseManagerApp/

├── src/
│   ├── App.java
│   ├── MainMenu.java
│   ├── ManagerCreation.java
│   ├── AddExpense.java
│   ├── AddTransfer.java
│   ├── Balance.java
│   ├── ExpenseManager.java
│   ├── PersonDAO.java
│   ├── ExpenseDAO.java
│   ├── Manager.java
│   ├── ManagerLoader.java
│   └── DatabaseConnection.java
├── bin/  (this folder will contain compiled .class files after compilation)
├── lib/
│   └── mariadb-java-client.jar  (MariaDB JDBC connector)
├── sql/
│   └── create_database.sql
├── .vscode/
│   └── (contains IDE-specific configuration files)
├── README.txt

SQL Database Initialization

Make sure to execute the create_database.sql script in MariaDB before launching the application. This script initializes the database and creates the necessary tables for the app.

SQL Script (create_database.sql) :

CREATE DATABASE IF NOT EXISTS expense_manager;

USE expense_manager;

CREATE TABLE IF NOT EXISTS person (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);

CREATE TABLE IF NOT EXISTS expense (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255),
    amount DECIMAL(10, 2),
    date DATE,
    payer_id INT,
    FOREIGN KEY (payer_id) REFERENCES person(id)
);

CREATE TABLE IF NOT EXISTS expense_participant (
    id INT AUTO_INCREMENT PRIMARY KEY,
    expense_id INT,
    person_id INT,
    amount_due DECIMAL(10, 2),
    FOREIGN KEY (expense_id) REFERENCES expense(id),
    FOREIGN KEY (person_id) REFERENCES person(id)
);

CREATE TABLE IF NOT EXISTS transfer (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) DEFAULT 'Reimbursement',
    amount DECIMAL(10, 2),
    from_person_id INT,
    to_person_id INT,
    date DATE,
    FOREIGN KEY (from_person_id) REFERENCES person(id),
    FOREIGN KEY (to_person_id) REFERENCES person(id)
);

