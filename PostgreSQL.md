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
- [Step 7: Create a New Table](#step-7-create-a-new-table)
- [Step 8: Grant Privileges to the New User](#step-8-grant-privileges-to-the-new-user)
- [Step 9: Connect as the New User](#step-9-connect-as-the-new-user)
- [Step 10: Useful PostgreSQL Commands](#step-10-useful-postgresql-commands)
- [Notes](#notes)

---

## Step 1: Install PostgreSQL

  - On Arch Linux:

    ```bash
    sudo pacman -Syu postgresql
    ```

  - On Ubuntu/Debian:

    ```bash
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

### Fix Locales
```bash
sudo nano /etc/locale.gen
```
Find and uncomment this line (remove the #): `en_US.UTF-8 UTF-8`
Save with `Ctrl+O`, then `Ctrl+X` to exit.

Generate Locales
```bash
sudo locale-gen
```
You should see:
```
Generating locales...
en_US.UTF-8... done
```

Set System Locale
```bash
sudo localectl set-locale LANG=en_US.UTF-8
```
Then try initializing cluster again.
---

## Step 3: Start and Enable PostgreSQL Service

On systemd-based systems:

```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

To check status:

```bash
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

- **When you try to switch user some errors might arise regarding password**
  
  - Solution:
    
    You must tell PostgreSQL to allow password login instead of peer login.
    
    - Step 1 – Open pg_hba.conf
    
      Run:
      
      ```pgsql
      sudo nano /var/lib/postgres/data/pg_hba.conf
      ```
      
      Find this block:
      
      ```pgsql
      # TYPE  DATABASE        USER            ADDRESS                 METHOD
      local   all             all                                     peer/trust
      ```
      
      Change `peer` or `trust` to `md5`. Save and exit.
      
      **Why this matters**
      
      | Mode | What happens |
      | --- | --- |
      | `peer` | Linux user must match DB user |
      | `trust` | No security. Anyone can access everything |
      | `md5` | Proper password-based authentication. |
      
    - Step 2 – Restart PostgreSQL
    
      ```bash
      sudo systemctl restart postgresql
      ```

    - Step 3 – Set password again (important)

      Now re-set the password to ensure it is stored with md5:

      ```pgsql
      sudo -u postgres psql
      ALTER USER myuser WITH PASSWORD 'your_password';
      \q
      ```

    - Step 4 – Test properly
    
      Now log in exactly how your app will:

      ```nginx
      psql -U task_user -d your_database -W
      ```
      
      Enter the password. You should now log in successfully.

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

## Step 7: Create a New Table

- Connect to your database

  ```sql
  \c myprojectdb
  ```

- Enable UUID Support

  ```sql
  CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
  ```

- Create a Table
  
  ```sql
  CREATE TABLE tablename (...);
  ```

There might be a error message

```makefile
ERROR: permission denied for schema public
```

**To solve this:**

- Login as postgres:
  
  ```bash
  psql -U postgres
  ```
  
- Connect to your DB:
  
  ```sql
  \c myprojectdb
  ```
  
- Then run:
  
  ```sql
  GRANT ALL ON SCHEMA public TO myuser;
  ALTER SCHEMA public OWNER TO myuser;
  \q
  ```
- Now reconnect as your app user:
  
  ```bash
  psql -U myuser -d myprojectdb
  ```
Now re-run the table creation commands.   

---

## Step 8: Grant Privileges to the New User

```sql
GRANT ALL PRIVILEGES ON DATABASE myprojectdb TO myuser;
```

---

## Step 9: Connect as the New User

From `psql` (inside `postgres` user):

```sql
\c myprojectdb myuser
```

Or directly from Linux shell:

```bash
psql -U myuser -d myprojectdb
```

---

## Step 10: Useful PostgreSQL Commands

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
