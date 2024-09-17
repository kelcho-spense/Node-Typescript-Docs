# MySQL

![mysql](mysql.png)

## MySQL Todo CRUD App

### **Prerequisites**
Make sure you have SQL Schema for `todo_tbl`:
Here’s the schema that corresponds to the table used in this code:

```SQL
    CREATE TABLE todo_tbl (
    id INT IDENTITY(1,1) PRIMARY KEY,
    task NVARCHAR(255) NOT NULL,
    completed BIT NOT NULL,
    created_at DATETIME DEFAULT GETDATE()
);

```

#### Step 1: Install Dependencies

```bash
npm install mysql2 typescript @types/node
```
download [MySQL Server](https://dev.mysql.com/downloads/installer/)

#### Step 2: MySQL Connection and CRUD Functions

Create a file `mysql-todo.ts`.

```javascript
import mysql, { PoolOptions, RowDataPacket, ResultSetHeader } from 'mysql2/promise';

// Define connection pool options
const access: PoolOptions = {
  host: 'localhost',
  user: 'admin',
  password: '@StrongPassword',
  database: 'todo_db',
};

// Create a connection pool
const pool = mysql.createPool(access);

// CRUD Operations using `todo_tbl`

// Create a Todo item in `todo_tbl`
async function createTodo(task: string, completed: boolean) {
  const [result] = await pool.execute<ResultSetHeader>(
    'INSERT INTO todo_tbl (task, completed, created_at) VALUES (?, ?, NOW())', 
    [task, completed]
  );
  console.log('Todo Created:', result);
}

// Fetch all Todos from `todo_tbl`
async function fetchAllTodos() {
  const [todos] = await pool.query<RowDataPacket[]>(
    'SELECT * FROM todo_tbl'
  );
  console.log('All Todos:', todos);
}

// Fetch Todo by ID from `todo_tbl`
async function fetchTodoById(id: number) {
  const [todo] = await pool.query<RowDataPacket[]>(
    'SELECT * FROM todo_tbl WHERE id = ?', 
    [id]
  );
  console.log('Todo by ID:', todo[0]);
}

// Delete Todo by ID from `todo_tbl`
async function deleteTodoById(id: number) {
  const [result] = await pool.execute<ResultSetHeader>(
    'DELETE FROM todo_tbl WHERE id = ?', 
    [id]
  );
  console.log('Todo deleted with ID:', id);
}

// Execute CRUD operations
(async () => {
  try {
    await createTodo('Learn TypeScript', false);
    await createTodo('Learn JavaScript', false);
    await createTodo('Learn C#', false);
    await fetchAllTodos();
    // Uncomment the following lines if you want to fetch or delete by ID
    // await fetchTodoById(1);
    // await deleteTodoById(1);
  } catch (error) {
    console.error('Error:', error);
  }
})();

```

#### Step 3: Run the App

```bash
npm run dev
```

---
