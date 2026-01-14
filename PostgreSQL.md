# PostgreSQL Setup Guide

This guide provides step-by-step commands to install PostgreSQL on Linux (Arch-based or similar), initialize it, create users and databases, and prepare it.

---

## Table of Contents

- [Step 1: Install PostgreSQL](#step-1-install-postgresql)
- [Step 2: Initialize PostgreSQL Database Cluster](#step-2-initialize-postgresql-database-cluster)
- [Step 3: Start and Enable PostgreSQL Service](#step-3-start-and-enable-postgresql-service)
- [Step 4: Access PostgreSQL Terminal](#step-4-access-postgresql-terminal)
- [Step 5: Create a New Database User](#step-5-create-a-new-database-user)
- [Step 6: Create a New Database](#step-6-create-a-new-database)
- [Step 7: Grant Privileges to the New User](#step-7-grant-privileges-to-the-new-user)
- [Step 8: Connect as the New User](#step-8-connect-as-the-new-user)
- [Step 9: Useful PostgreSQL Commands](#step-9-useful-postgresql-commands)
- [Notes](#notes)

---

## Step 1: Install PostgreSQL

  - On Arch Linux:

    ```bash
    sudo pacman -Syu postgresql
    ```

  - On Ubuntu/Debian:

    ```
    sudo apt update
    sudo apt install postgresql postgresql-contrib
    ```

Installation automatically creates a user named postgres. You can login to that user

```bash
sudo -iu postgres
```

---

## Step 2: Initialize PostgreSQL Database Cluster

PostgreSQL needs a data directory to store databases. So you need to initialize a cluster.

```bash
initdb --locale en_US.UTF-8 -D /var/lib/postgres/data
```
- `initdb` initializes a brand-new PostgreSQL database cluster.

- `--locale en_US.UTF-8` sets language, sorting rules, and encoding.

- `-D` specifies where database files will live.


This command should create the cluster. To check use `ls` and you should see the **data** folder.

---

## Step 3: Start and Enable PostgreSQL Service

On systemd-based systems:

```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

To check status:

```
sudo systemctl status postgresql
```

---

## Step 4: Access PostgreSQL Terminal

Switch to your user(in my case postgres) and start psql:

```bash
sudo su - postgres
psql
```

You should see:

```makefile
postgres=#
```

---

## Step 5: Create a New Database User

Inside `psql`:

```sql
CREATE USER myuser WITH PASSWORD 'mypassword';
```

- Replace `myuser` and `mypassword` with your desired credentials.

---

## Step 6: Create a New Database

```sql
CREATE DATABASE myprojectdb;
```

---

## Step 7: Grant Privileges to the New User

```sql
GRANT ALL PRIVILEGES ON DATABASE myprojectdb TO myuser;
```

---

## Step 8: Connect as the New User

From `psql` (inside `postgres` user):

```sql
\c myprojectdb myuser
```

Or directly from Linux shell:

```bash
psql -U myuser -d myprojectdb
```

---

## Step 9: Useful PostgreSQL Commands

| Command | Description |
| --- | --- |
| `\l` | List all databases |
| `\du` | List all users/roles |
| `\dt` | List all tables in the current database |
| `\q`  | Quit `psql` |
| `CREATE TABLE table_name (...);` | Create a table |
| `INSERT INTO table_name VALUES (...);` | Insert data |
| `SELECT * FROM table_name;` | Query data |

---

## Notes

- Always avoid running PostgreSQL as root.
- Store passwords securely in environment variables for your app.
- For production, consider enabling SSL for remote connections.
