# Storing Users

## 1. Overview

Users in the Library Management System have:

- A **unique ID** (e.g., UUID).
- **Basic details**: `name` (string), `email` (string).
- A **membership type**: one of `["student", "basic", "premium"]`.

Each membership type enforces different borrowing limits (books, magazines, DVDs, CDs). You’ll integrate these rules in later pages when implementing borrowing logic. For now, focus on **storing user data** and providing CRUD endpoints (if needed).

---

## 2. Data Model

### 2.1 Minimal Fields

1. `user_id`: Unique identifier (UUID or auto-increment integer).  
2. `name`: User’s full name (string).  
3. `email`: User’s email (string).  
4. `membership_type`: `"student"`, `"basic"`, or `"premium"`.

### 2.2 Optional Fields

- `created_at`: Timestamp of user creation (useful for auditing/logs).
- `updated_at`: Timestamp of last update.

> **Note**: Depending on your language or framework, you might use classes (`class User`), structs, or just plain objects. If you’re using a SQL database, you’ll likely define a `users` table.

---

## 3. Database Options

### 3.1 Storing Users with SQLite

If you’re choosing **SQLite**:
- Create a `.db` file in your Docker container or on your local machine.
- Define a `users` table, for example:

  ```sql
  CREATE TABLE users (
    user_id TEXT PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT NOT NULL,
    membership_type TEXT NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
  );
  ```

- **Pros**: Self-contained, no extra Docker services needed.  
- **Cons**: Limited concurrency for large-scale apps.

### 3.2 Storing Users with Postgres/MySQL (Docker Compose)

If you prefer a more robust DB (Postgres, MySQL, etc.):

- Define your `docker-compose.yml` with both `app` and `db` services.  
- Create a `users` table in your DB. For example, in PostgreSQL:

  ```sql
  CREATE TABLE users (
    user_id UUID PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    membership_type VARCHAR(50) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  );
  ```

- Make sure your app container can **connect** to the database container using environment variables (e.g., `DB_HOST`, `DB_USER`, etc.).

### 3.3 In-Memory or JSON File (for Prototyping)

- If you’re just getting started or want a **quick prototype**, you can store users in a simple array or JSON file:

  ```js
  // Example for Node.js
  const users = [];

  function addUser(user) {
    users.push(user);
  }
  ```

- **Warning**: This won’t persist data after your application restarts (unless you’re reading/writing to a file). It’s fine for local practice but less realistic.

> **Tip for Junior/Mid-Level**: If you’re new to SQL, try SQLite first; if you’re comfortable spinning up multi-container setups, go for Postgres or MySQL.  

---

## 4. Creating Users (Endpoint Design)

### 4.1 `POST /users`

**Goal**: Create a new user record in your data store (DB, in-memory, etc.).

1. **Request Body** example:
   ```json
   {
     "name": "Alice",
     "email": "alice@example.com",
     "membership_type": "student"
   }
   ```
2. **Validation**:
   - Check `name` and `email` are provided.
   - Check `membership_type` is one of `["student", "basic", "premium"]`.
3. **Implementation**:
   - Generate a unique ID (UUID or auto increment).
   - Insert the user into the **users** table (SQL) or array/file (non-SQL).
4. **Response** example:
   ```json
   {
     "user_id": "123e4567-e89b-12d3-a456-426614174000",
     "name": "Alice",
     "email": "alice@example.com",
     "membership_type": "student"
   }
   ```
5. **Status Codes**:
   - **201 Created** if successful.
   - **400 Bad Request** if validation fails.
   - **409 Conflict** if you want to handle duplicate emails with a special code (optional).

---

## 5. Reading Users (Optionally Expose More Endpoints)

Depending on your requirements, you may also provide endpoints to list or retrieve user details:

### 5.1 `GET /users`

- **Description**: Returns a list of all users.
- **Response**: An array of user objects.

### 5.2 `GET /users/:id`

- **Description**: Returns a single user by `user_id`.
- **Response**: The user object, or a 404 if not found.

---

## 6. Updating or Deleting Users (Optional)

You might want to allow membership changes or the ability to delete users entirely. Decide if that’s relevant to your library system.

### 6.1 `PUT /users/:id` (Update)

- **Request Body** could include fields like `membership_type` if you allow changes.
- **Response**: The updated user.

### 6.2 `DELETE /users/:id`

- **Response**: Some success message if the user was removed, or 404 if not found.

---

## 7. Ensuring Data Integrity & Best Practices

1. **Unique Email**: Consider making `email` unique in your DB. Helps avoid duplicate accounts.
2. **Indexing**: If using SQL, index your `email` or `membership_type` if queries rely on them often.
3. **Timestamps**: `created_at` and `updated_at` can be automatically managed by your DB (using default values, triggers, or ORM features).
4. **Validation**: Always validate membership types to avoid data inconsistencies (`"student"`, `"basic"`, `"premium"` only).

---

## 8. Docker & Database Integration

### 8.1 SQLite in One Container

- Add SQLite to your Docker image (e.g., for Node.js: `RUN npm install sqlite3`).
- Store the `.db` file inside your container’s filesystem.  
- **Pros**: Single container, no Compose needed.

### 8.2 Docker Compose with Postgres (Example)

- Add a `db` service to your `docker-compose.yml`.  
- In your `app` service, set environment variables (`DB_HOST=db`, `DB_NAME=library`, etc.).
- **Migrate** or create tables automatically on startup (using migration tools or custom scripts).

---

## 9. Testing User Storage

Before moving on to item storage or borrowing logic, **test** your user endpoints thoroughly:

1. **POST /users** to create new users.
2. **GET /users`** or `GET /users/:id` to confirm user data is stored correctly.
3. (Optional) **PUT /users/:id** to edit user membership.
4. (Optional) **DELETE /users/:id** if supporting user removal.

Use a tool like **Postman**, **Insomnia**, or just **curl** to verify correct responses (including status codes).

---

## 10. Conclusion

You have now laid a **solid foundation** for user management in your Library Management System. With **unique user records** and basic membership types in place, you’re ready to integrate the **next component**: **storing items** (books, magazines, DVDs, CDs) and eventually handling the **borrowing** logic that ties users to items.

**Next Page**: [Storing Items & Basic Catalog Management](#)  
*(Link to the next part of the instructions where you define item data models and implement endpoints for adding and viewing library items.)*

---

**Recap**: On this page, you learned how to:
- Create a `users` table or data structure.
- Implement an endpoint to add new users.
- Optionally retrieve/update/delete users as needed.
- Integrate database logic with Docker or Docker Compose.

With your users properly stored, you can move on to the **item** side of the library and eventually combine these to manage borrowing and returning.