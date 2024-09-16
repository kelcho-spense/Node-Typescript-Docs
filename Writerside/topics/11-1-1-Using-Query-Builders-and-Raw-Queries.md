# 11.1.1 Using Query Builders and Raw Queries

### 1. MySQL Todo CRUD App

#### Step 1: Install Dependencies

```bash
npm install mysql2 typescript @types/node
```

#### Step 2: MySQL Connection and CRUD Functions

Create a file `mysql-todo.ts`.

```typescript
import { createConnection } from 'mysql2/promise';

// MySQL Connection
const connection = createConnection({
  host: 'localhost',
  user: 'root',
  password: 'password',
  database: 'todo_db',
});

async function createTodo(title: string) {
  const conn = await connection;
  const [result] = await conn.execute('INSERT INTO todos (title) VALUES (?)', [title]);
  console.log('Todo Created:', result);
}

async function fetchAllTodos() {
  const conn = await connection;
  const [todos] = await conn.query('SELECT * FROM todos');
  console.log('All Todos:', todos);
}

async function fetchTodoById(id: number) {
  const conn = await connection;
  const [todo] = await conn.query('SELECT * FROM todos WHERE id = ?', [id]);
  console.log('Todo by ID:', todo[0]);
}

async function deleteTodoById(id: number) {
  const conn = await connection;
  await conn.execute('DELETE FROM todos WHERE id = ?', [id]);
  console.log('Todo deleted with ID:', id);
}

// Execute CRUD operations
(async () => {
  await createTodo('Learn TypeScript');
  await fetchAllTodos();
  await fetchTodoById(1);
  await deleteTodoById(1);
})();
```

#### Step 3: Run the App

```bash
tsc mysql-todo.ts && node mysql-todo.js
```

---

### 2. PostgreSQL Todo CRUD App

#### Step 1: Install Dependencies

```bash
npm install pg typescript @types/node
```

#### Step 2: PostgreSQL Connection and CRUD Functions

Create a file `postgres-todo.ts`.

```javascript
import { Pool } from 'pg';

// PostgreSQL Connection
const pool = new Pool({
  user: 'postgres',
  host: 'localhost',
  database: 'todo_db',
  password: 'password',
  port: 5432,
});

async function createTodo(title: string) {
  const result = await pool.query('INSERT INTO todos (title) VALUES ($1) RETURNING *', [title]);
  console.log('Todo Created:', result.rows[0]);
}

async function fetchAllTodos() {
  const result = await pool.query('SELECT * FROM todos');
  console.log('All Todos:', result.rows);
}

async function fetchTodoById(id: number) {
  const result = await pool.query('SELECT * FROM todos WHERE id = $1', [id]);
  console.log('Todo by ID:', result.rows[0]);
}

async function deleteTodoById(id: number) {
  await pool.query('DELETE FROM todos WHERE id = $1', [id]);
  console.log('Todo deleted with ID:', id);
}

// Execute CRUD operations
(async () => {
  await createTodo('Learn PostgreSQL');
  await fetchAllTodos();
  await fetchTodoById(1);
  await deleteTodoById(1);
})();
```

#### Step 3: Run the App

```bash
tsc postgres-todo.ts && node postgres-todo.js
```

---

### 3. MongoDB Todo CRUD App

#### Step 1: Install Dependencies

```bash
npm install mongoose typescript @types/node
```

#### Step 2: MongoDB Connection and CRUD Functions

Create a file `mongo-todo.ts`.

```javascript
import mongoose from 'mongoose';

// MongoDB Connection
mongoose.connect('mongodb://localhost:27017/todo_db', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

const todoSchema = new mongoose.Schema({
  title: String,
});

const Todo = mongoose.model('Todo', todoSchema);

async function createTodo(title: string) {
  const todo = new Todo({ title });
  await todo.save();
  console.log('Todo Created:', todo);
}

async function fetchAllTodos() {
  const todos = await Todo.find();
  console.log('All Todos:', todos);
}

async function fetchTodoById(id: string) {
  const todo = await Todo.findById(id);
  console.log('Todo by ID:', todo);
}

async function deleteTodoById(id: string) {
  await Todo.findByIdAndDelete(id);
  console.log('Todo deleted with ID:', id);
}

// Execute CRUD operations
(async () => {
  await createTodo('Learn MongoDB');
  await fetchAllTodos();
  const todo = await Todo.findOne(); // Assuming one exists
  if (todo) {
    await fetchTodoById(todo._id.toString());
    await deleteTodoById(todo._id.toString());
  }
})();
```

#### Step 3: Run the App

```bash
tsc mongo-todo.ts && node mongo-todo.js
```

---

### 4. MSSQL Todo CRUD App

#### Step 1: Install Dependencies

```bash
npm install mssql typescript @types/node
```

#### Step 2: MSSQL Connection and CRUD Functions

Create a file `mssql-todo.ts`.

```javascript
import sql from 'mssql';

// MSSQL Connection
const config = {
  user: 'sa',
  password: 'password',
  server: 'localhost',
  database: 'todo_db',
  options: {
    encrypt: false, // for local dev
  },
};

const poolPromise = sql.connect(config);

async function createTodo(title: string) {
  const pool = await poolPromise;
  const result = await pool.request().query(`INSERT INTO todos (title) VALUES ('${title}')`);
  console.log('Todo Created:', result);
}

async function fetchAllTodos() {
  const pool = await poolPromise;
  const result = await pool.request().query('SELECT * FROM todos');
  console.log('All Todos:', result.recordset);
}

async function fetchTodoById(id: number) {
  const pool = await poolPromise;
  const result = await pool.request().query(`SELECT * FROM todos WHERE id = ${id}`);
  console.log('Todo by ID:', result.recordset[0]);
}

async function deleteTodoById(id: number) {
  const pool = await poolPromise;
  await pool.request().query(`DELETE FROM todos WHERE id = ${id}`);
  console.log('Todo deleted with ID:', id);
}

// Execute CRUD operations
(async () => {
  await createTodo('Learn MSSQL');
  await fetchAllTodos();
  await fetchTodoById(1);
  await deleteTodoById(1);
})();
```

#### Step 3: Run the App

```bash
tsc mssql-todo.ts && node mssql-todo.js
```

---

### Summary

In each of the examples above:
- We defined connection configurations for each database.
- Implemented CRUD operations (`createTodo`, `fetchAllTodos`, `fetchTodoById`, `deleteTodoById`).
- The functions are called with dummy data, and the operations are printed to the console.

These examples are basic and do not involve Express.js or routing. Instead, they are designed to be standalone scripts that perform CRUD operations directly on the respective databases. You can adapt this structure to your needs, including adding error handling, more detailed data validation, and additional features.