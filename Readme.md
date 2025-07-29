# Introduction to Databases

## What is a Database?
A database is a structured system for storing, managing, and retrieving data. It organizes data in a way that makes it easy to access, update, and manage efficiently using software called a Database Management System (DBMS). Databases are critical in applications like websites, banking systems, and inventory management, ensuring data is stored securely and retrieved quickly.

## Types of Databases
### Relational Databases
- **Structure**: Data is stored in tables, with each table containing rows (records) and columns (attributes). Tables are linked through keys.
- **Key Feature**: Use Structured Query Language (SQL) for defining and manipulating data.
- **Use Cases**: Ideal for structured data with clear relationships, like financial records or customer data.
- **Examples**:
  - **MySQL**: Open-source, widely used for web applications.
  - **PostgreSQL**: Advanced open-source database with strong standards compliance.
  - **Oracle Database**: Enterprise-level, used in large-scale applications.

### NoSQL Databases
- **Structure**: Designed for unstructured or semi-structured data, offering flexibility in data storage.
- **Types**:
  - **Document-based**: Stores data as JSON or BSON documents (e.g., MongoDB).
  - **Key-Value**: Simple key-value pairs for fast retrieval (e.g., Redis).
  - **Column-Family**: Organizes data into columns instead of rows for large-scale analytics (e.g., Cassandra).
  - **Graph**: Focuses on relationships between data points, ideal for social networks or fraud detection (e.g., Neo4j).
- **Use Cases**: Big data, real-time analytics, or applications with dynamic data structures.
- **Examples**:
  - **MongoDB**: Popular for handling JSON-like documents.
  - **Redis**: In-memory database for caching and real-time applications.

## Core Database Concepts
### Tables
- The foundation of relational databases.
- Columns define the data attributes (e.g., `name`, `age`), each with a specific data type (e.g., VARCHAR, INT).
- Rows represent individual records or entries.
- Example: A `customers` table might have columns `customer_id`, `name`, and `email`, with each row representing a unique customer.

### Primary Key
- A unique identifier for each record in a table.
- Ensures no two rows have the same identifier (e.g., `customer_id`).
- Cannot be null and must be unique to maintain data integrity.
- Example: In a `users` table, `user_id` could be the primary key.

### Foreign Key
- A column in one table that references the primary key of another table.
- Establishes relationships between tables (e.g., a `orders` table with a `customer_id` foreign key linking to the `customers` table).
- Enforces referential integrity, ensuring valid relationships.

### Indexes
- Special database structures that improve query performance.
- Example: An index on `email` in a `users` table speeds up searches for users by email.
- Trade-off: Indexes speed up reads but can slow down writes (inserts/updates).

### Normalization
- A process to eliminate data redundancy and ensure data integrity.
- Involves splitting data into related tables and using keys to link them.
- Example: Instead of storing customer details in every order, store them in a `customers` table and reference them in an `orders` table via `customer_id`.

## SQL Example
Here’s a more detailed SQL example demonstrating table creation, data insertion, and querying with a relationship between two tables:

```sql
-- Create a customers table
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE
);

-- Create an orders table with a foreign key
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    amount DECIMAL(10, 2),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

-- Insert data into customers
INSERT INTO customers (customer_id, name, email)
VALUES (1, 'Jane Smith', 'jane@example.com'),
       (2, 'John Doe', 'john@example.com');

-- Insert data into orders
INSERT INTO orders (order_id, customer_id, order_date, amount)
VALUES (101, 1, '2025-07-29', 199.99),
       (102, 2, '2025-07-30', 49.50);

-- Query: Join tables to get customer orders
SELECT c.name, o.order_id, o.order_date, o.amount
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;
```

**Output** (example):
| name        | order_id | order_date  | amount  |
|-------------|----------|-------------|---------|
| Jane Smith  | 101      | 2025-07-29  | 199.99  |
| John Doe    | 102      | 2025-07-30  | 49.50   |

## Database Management Systems (DBMS)
A DBMS is software that manages databases, handling tasks like data storage, retrieval, security, and backups. It acts as an intermediary between the database and users or applications.
- **Examples**:
  - **MySQL**: Lightweight, widely used for web apps.
  - **PostgreSQL**: Supports advanced features like JSON storage and full-text search.
  - **MongoDB**: NoSQL, great for flexible, document-based data.
  - **Oracle**: Enterprise-grade, used in large organizations.
  - **SQL Server**: Microsoft’s DBMS, integrated with Windows ecosystems.
- **Functions**:
  - Define database structure (schemas).
  - Handle queries and transactions.
  - Ensure data consistency and security.

## NoSQL Example (MongoDB)
For contrast, here’s an example of a MongoDB document (NoSQL) for a similar customer-order scenario:

```javascript
// Insert a customer document with embedded orders
db.customers.insertOne({
    customer_id: 1,
    name: "Jane Smith",
    email: "jane@example.com",
    orders: [
        { order_id: 101, order_date: "2025-07-29", amount: 199.99 },
        { order_id: 103, order_date: "2025-07-30", amount: 89.00 }
    ]
});

// Query to find customer and their orders
db.customers.find({ customer_id: 1 }).pretty();
```

**Output** (simplified):
```json
{
    "customer_id": 1,
    "name": "Jane Smith",
    "email": "jane@example.com",
    "orders": [
        { "order_id": 101, "order_date": "2025-07-29", "amount": 199.99 },
        { "order_id": 103, "order_date": "2025-07-30", "amount": 89.00 }
    ]
}
```

## Best Practices
1. **Normalization**: Reduce redundancy by organizing data into related tables (applies to relational databases).
2. **Indexing**: Create indexes on frequently queried columns, but avoid over-indexing to prevent write performance issues.
3. **Security**: Use strong authentication, encrypt sensitive data, and limit access to authorized users.
4. **Backups**: Schedule regular backups to prevent data loss.
5. **Query Optimization**: Write efficient queries (e.g., avoid `SELECT *`, use proper joins).
6. **Scalability**: Choose a database type (SQL vs. NoSQL) based on your application’s needs (e.g., relational for structured data, NoSQL for flexibility).
7. **Data Validation**: Enforce constraints (e.g., NOT NULL, UNIQUE) to ensure data quality.

## Additional Considerations
- **ACID Properties** (for relational databases):
  - **Atomicity**: Ensures transactions are all-or-nothing.
  - **Consistency**: Maintains data integrity after transactions.
  - **Isolation**: Ensures transactions don’t interfere with each other.
  - **Durability**: Guarantees committed transactions are saved.
- **Scalability**:
  - **Vertical Scaling**: Add more power (CPU, RAM) to a single server.
  - **Horizontal Scaling**: Add more servers (common in NoSQL databases).
- **CAP Theorem** (for distributed databases):
  - A database can only guarantee two of three properties: Consistency, Availability, Partition Tolerance.
  - Example: NoSQL databases like Cassandra prioritize availability and partition tolerance over immediate consistency.

## When to Use Which Database
- **Relational**: Use for structured data with clear relationships, like financial systems or CRM applications.
- **NoSQL**: Use for large-scale, unstructured, or rapidly changing data, like social media feeds or IoT data.
- **Hybrid Needs**: Some modern databases (e.g., PostgreSQL with JSONB) support both structured and unstructured data.
