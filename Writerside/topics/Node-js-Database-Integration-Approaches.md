# 11.1 Node.js Database Integration Approaches

There are two main approaches for integrating Node.js with databases:

#### 1. **Using Query Builders and Raw Queries**

For developers who prefer greater control over SQL queries or who need to work directly with database-specific features, raw queries or query builders like **Knex.js** are great options. These allow developers to write SQL queries directly or through a builder that generates SQL dynamically.

##### Example: Raw Query with MySQL
```typescript
const mysql = require('mysql2/promise');

async function fetchTodos() {
  const connection = await mysql.createConnection({ /* connection details */ });
  const [rows] = await connection.execute('SELECT * FROM todos');
  return rows;
}
```

#### 2. **Using ORM (Object-Relational Mapping)**

ORMs abstract database interactions, allowing developers to work with objects and models instead of raw SQL queries. This approach is particularly useful for those who want to avoid writing raw SQL and focus on the business logic.

Popular Node.js ORMs:
- **Drizzle** : It’s the only ORM with both relational and SQL-like query APIs, providing you the best of both worlds when it comes to accessing your relational data.
- **Sequelize**: A promise-based ORM for MySQL, PostgreSQL, and SQLite that provides a comprehensive set of features, including model definition, associations, and transactions.
- **TypeORM**: A versatile ORM that supports a wide variety of databases, including MySQL, PostgreSQL, SQLite, and MongoDB. It works well with TypeScript and supports decorators for defining models.
- **Mongoose**: A popular ODM (Object Data Modeling) library for MongoDB that allows for defining schemas, models, and relationships in a simple and efficient way.
