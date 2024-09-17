# PostgreSQL
![postgres](postgres.png)

## MySQL Todo CRUD App

### **Prerequisites**
Make sure you have SQL Schema for `todo_tbl`: Here’s the schema that corresponds to the table used in this code:

```SQL
CREATE TABLE todo_tbl (
  id SERIAL PRIMARY KEY,              -- SERIAL auto-increments the ID
  task VARCHAR(255) NOT NULL,         -- Task is a string with a max length of 255 characters
  completed BOOLEAN DEFAULT FALSE,    -- Boolean, default is FALSE
  created_at TIMESTAMPTZ DEFAULT NOW()  -- Timestamp with timezone, defaulting to the current timestamp
);
```

#### Step 1: Install Dependencies

```bash
npm install pg 
npm install typescript @types/node @types/pg --save-dev
```

download the [PostgresSQL Server](https://www.postgresql.org/download/)

#### Step 2: PostgreSQL Connection and CRUD Functions

Create a file `index.ts`.

```javascript
import { Pool } from 'pg';

// PostgreSQL Connection
const pool = new Pool({
  user: 'admin',
  host: 'localhost',
  database: 'todo_db',
  password: '@StrongPassword',
  port: 5432,
});

// Create a new Todo item in the `todo_tbl` table
async function createTodo(task: string, completed: boolean) {
  const result = await pool.query(
    'INSERT INTO todo_tbl (task, completed, created_at) VALUES ($1, $2, NOW()) RETURNING *',
    [task, completed]
  );
  console.log('Todo Created:', result.rows[0]);
}

// Fetch all Todos from the `todo_tbl` table
async function fetchAllTodos() {
  const result = await pool.query('SELECT * FROM todo_tbl');
  console.log('All Todos:', result.rows);
}

// Fetch a Todo by ID from the `todo_tbl` table
async function fetchTodoById(id: number) {
  const result = await pool.query('SELECT * FROM todo_tbl WHERE id = $1', [id]);
  console.log('Todo by ID:', result.rows[0]);
}

// Delete a Todo by ID from the `todo_tbl` table
async function deleteTodoById(id: number) {
  await pool.query('DELETE FROM todo_tbl WHERE id = $1', [id]);
  console.log('Todo deleted with ID:', id);
}

// Execute CRUD operations
(async () => {
  await createTodo('Learn PostgreSQL', false);
  await fetchAllTodos();
  await fetchTodoById(1);
  // await deleteTodoById(1);
})();

```

#### Step 3: Run the App

```bash
npm run dev
```

---
