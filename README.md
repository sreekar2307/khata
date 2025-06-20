# Khata – Splitwise-like Expense Sharing App

Katha is a backend service for managing shared expenses, inspired by Splitwise. It allows users to add expenses, split them by amount or percentage, and view their balances with others. The project is written in Go and uses Gin for HTTP, GORM for ORM, and PostgreSQL as the database.

---

## Features

- **User Authentication** (JWT-based)
- **Add Expenses** (split equally, by amount, or by percentage)
- **View All Your Expenses**
- **View Balances** (summary of what you owe and are owed)
- **Seed and Migrate Database**
- **Extensible Split and Simplification Logic**

---

## Project Structure

- `cmd/` – Application entry points and CLI commands (run server, migrate, seed, generate token)
- `controller/` – HTTP controllers and routing
- `service/` – Business logic
- `repository/` – Database access layer
- `model/` – Data models and types
- `splitter/` – Logic for splitting expenses
- `simplifier/` – Logic for simplifying debts
- `db/` – Database connection and setup
- `config/` – Configuration loading

---

## Getting Started

### Prerequisites

- Go 1.24+
- Docker (for running PostgreSQL)

### Setup Local

1. **Clone the repository:**
   ```sh
   git clone https://github.com/sreekar2307/khata.git 
   cd khata
   ```

2. **Start PostgreSQL using Docker Compose:**
   ```sh
   docker compose up -d postgres
   ```

3. **Configure the app:**
   - Edit `config.yaml` if you need to change DB credentials or server settings. it should not be necessary if you are using the default Docker setup.

4. **Run database migrations:**
   ```sh
   make migrate 
   ```

5. **Seed the database with test users:**
   ```sh
   make seed
   ```

6. **Start the HTTP server:**
   ```sh
   make http
   ```

### Setup Docker

1. **Clone the repository:**
   ```sh
   git clone https://github.com/sreekar2307/khata.git 
   cd khata
   ```
2. **Start all the services:**
   ```sh
   docker compose up -d --build
   ```
3. **Run the migrations**
   ```sh
   docker compose run khata make migrate
   ```
4. **Seed the database with test users:**
   ```sh
   docker compose run khata make seed
   ```
---

## Authentication

- Generate a JWT token for a user:
   - locally:
     ```sh
     make token user=john.doe@email.com password=password
     ```
    - in Docker:
      ```sh
      docker compose run khata make token user=john.doe@email.com password=password
      ```
- Use the returned token as a Bearer token in the `Authorization` header for all API requests.
- Prefix the command 

---

## Database Schema

### Users Table
- `id` (uint64, primary key)
- `email` (string, unique)
- `password_hash` (string)

### Expenses Table
- `id` (uint64, primary key)
- `description` (string)
- `amount` (uint64) // as paise (smallest unit)
- `lender_id` (uint64, fk Users)

### Ledgers Table
- `id` (uint64, primary key)
- `expense_id` (uint64, fk to Expenses)
- `lender_id` (uint64, fk to Users)
- `borrower_id` (uint64, fk to Users)
- `amount` (uint64) // as paise (smallest unit)

---

## Configuration

Edit `config.yaml` to change server or database settings.

---

## Postman Collection

[Public Collection](https://github.com/sreekar2307/katha/blob/main/postman/katha.postman_collection.json)
[Local Environment](https://github.com/sreekar2307/katha/blob/main/postman/local.postman_environment.json)

