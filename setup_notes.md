# Odoo Setup Guide - Clean Architecture Approach

## Prerequisites
- macOS with Postgres.app installed
- Python 3.12+
- Git

## Step-by-Step Setup

### 1. Clone Odoo Repository
```bash
git clone https://github.com/odoo/odoo.git
cd odoo
git checkout 18.0  # Use stable branch
```

### 2. Create Python Virtual Environment
```bash
python3 -m venv odoo-venv
source odoo-venv/bin/activate
```

### 3. Install Python Dependencies
```bash
pip install --upgrade pip
pip install wheel
pip install -r requirements.txt
```

### 4. Setup PostgreSQL Database

#### Start Postgres.app
- Open Postgres.app from Applications
- Click "Start" button

#### Verify PostgreSQL is running
```bash
pg_isready
# Should return: /tmp:5432 - accepting connections
```

#### Create Database User and Database
```bash
# Create Odoo user
createuser -d -R -S odoo

# Create database
createdb -O odoo odoo_learning

# Verify database was created
psql -l
```

### 5. Create Odoo Configuration File

Create `odoo.conf` in the root odoo directory:

```ini
[options]
# Database settings
db_host = localhost
db_port = 5432
db_user = odoo
db_password = 
db_name = odoo_learning

# Server settings
http_port = 8069
workers = 0

# Paths
addons_path = addons

# Logging
log_level = info
log_db = False

# Security (development only)
admin_passwd = admin123

# Performance
limit_memory_hard = 2684354560
limit_memory_soft = 2147483648
```

### 6. Initialize Odoo Database

#### With Demo Data (for learning)
```bash
source odoo-venv/bin/activate
python3 odoo-bin -c odoo.conf --init=base --stop-after-init
```

#### Without Demo Data (clean start)
```bash
source odoo-venv/bin/activate
python3 odoo-bin -c odoo.conf --init=base --without-demo=all --stop-after-init
```

### 7. Start Odoo
```bash
source odoo-venv/bin/activate
python3 odoo-bin -c odoo.conf
```

### 8. Access Odoo
- URL: http://localhost:8069
- Login: admin / admin

## Currency Setup (Important!)

If you need non-USD currency:
- **MUST** be done during initial setup
- Or recreate database without demo data
- Demo data locks currency to USD

## Troubleshooting

### PostgreSQL Issues
```bash
# Check if PostgreSQL is running
pg_isready

# Kill processes using port 5432
sudo lsof -ti:5432 | xargs kill -9

# Reset PostgreSQL data (nuclear option)
rm -rf ~/Library/Application\ Support/Postgres/var-*
# Then restart Postgres.app
```

### Database Issues
```bash
# Check database exists
psql -l

# Check if tables were created
psql -U odoo -d odoo_learning -c "SELECT count(*) FROM information_schema.tables WHERE table_schema = 'public';"

# Drop and recreate database
dropdb -U odoo odoo_learning
createdb -U odoo odoo_learning
```

### Common Errors

#### "Database not initialized"
```bash
python3 odoo-bin -c odoo.conf --init=base --stop-after-init
```

#### "Port 5432 in use"
```bash
sudo lsof -ti:5432 | xargs kill -9
```

#### Wrong database connection
- Check `db_name` parameter in odoo.conf (not `database`)
- Verify PostgreSQL user exists: `psql -U odoo -d odoo_learning`

## Clean Architecture Notes

Following the philosophy from CLAUDE.md:
- Odoo = Data platform only
- Business logic goes in external FastAPI services
- Use readonly fields to store external service results
- Focus on API integration, not heavy customization

## Next Steps for Learning

1. Install basic modules: Contacts, Sales, Accounting
2. Explore data structure via Settings ’ Technical
3. Learn JSON-RPC API calls
4. Build external FastAPI services
5. Practice clean integration patterns