# 11.1.2 Using ORM (Object-Relational Mapping)

ORMs abstract database interactions, allowing developers to work with objects and models instead of raw SQL queries. This approach is particularly useful for those who want to avoid writing raw SQL and focus on the business logic.

Popular Node.js ORMs:
- **Sequelize**: A promise-based ORM for MySQL, PostgreSQL, and SQLite that provides a comprehensive set of features, including model definition, associations, and transactions.
- **TypeORM**: A versatile ORM that supports a wide variety of databases, including MySQL, PostgreSQL, SQLite, and MongoDB. It works well with TypeScript and supports decorators for defining models.
- **Mongoose**: A popular ODM (Object Data Modeling) library for MongoDB that allows for defining schemas, models, and relationships in a simple and efficient way.

##### Example: Using Sequelize with PostgreSQL
```typescript
import { Sequelize, DataTypes } from 'sequelize';

const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: 'postgres'
});

const Todo = sequelize.define('Todo', {
  title: {
    type: DataTypes.STRING,
    allowNull: false,
  },
});

async function fetchTodos() {
  const todos = await Todo.findAll();
  return todos;
}
```

### Common CRUD Operations in Node.js with Databases

CRUD (Create, Read, Update, Delete) operations are the foundation of interacting with databases. These operations allow you to store, retrieve, update, and delete data.

- **Create**: Inserting new records into a table (SQL) or collection (NoSQL).
- **Read**: Querying records from a database.
- **Update**: Modifying existing records in the database.
- **Delete**: Removing records from the database.

##### Example: CRUD with MongoDB (Mongoose)
```javascript
import mongoose from 'mongoose';

const todoSchema = new mongoose.Schema({ title: String });
const Todo = mongoose.model('Todo', todoSchema);

// Create a new Todo
async function createTodo() {
  const todo = new Todo({ title: 'Learn Node.js with MongoDB' });
  await todo.save();
  console.log('Todo created:', todo);
}

// Read all Todos
async function fetchTodos() {
  const todos = await Todo.find();
  console.log('All Todos:', todos);
}
```

### Best Practices for Database Integration with Node.js

1. **Connection Pooling**: Use connection pools to reuse connections efficiently rather than creating a new one for each request, which can improve performance and resource usage.
2. **Error Handling**: Ensure proper error handling for database queries and connections to avoid application crashes. Implement retry logic or graceful shutdown in case of failures.
3. **Security**: Always secure database credentials using environment variables and avoid hardcoding sensitive information in your codebase. Additionally, ensure SQL queries are parameterized to prevent SQL injection attacks.
4. **Query Optimization**: For large datasets, optimize queries to reduce latency. This can involve using indexing, caching frequently accessed data, and minimizing the use of `SELECT *` queries.

