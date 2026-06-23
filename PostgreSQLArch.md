## Step 1 — Install PostgreSQL
```bash
sudo pacman -S postgresql
```

## Step 2 — Initialize the Database
```bash
sudo -u postgres initdb -D /var/lib/postgres/data
```

## Step 3 — Start & Enable PostgreSQL
```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql  # auto-start on boot
```

Verify it's running:
```bash
sudo systemctl status postgresql
```
You should see active (running) in green ✅

## Step 4 — Create the Database
```bash
sudo -u postgres psql
```

You're now inside psql. Run:
```sql
CREATE DATABASE nestor;
CREATE USER nestor_user WITH PASSWORD 'nestorpass';
GRANT ALL PRIVILEGES ON DATABASE nestor TO nestor_user;
\q
```

## Step 5 — Test the Connection
```bash
psql -U nestor_user -d nestor -h localhost
```

If you see varaden=> you're in ✅
Type `\q` to exit.
