# Simple Login & Registration System

A basic PHP login/registration system with MySQL database.

## Files Overview

- `config.php` - Database connection configuration
- `index.php` - Login/Register page
- `register.php` - Registration handler
- `login.php` - Login handler
- `dashboard.php` - Protected dashboard page
- `logout.php` - Logout handler
- `style.css` - Styling
- `database.sql` - Database schema

## Local Setup

### Prerequisites
- PHP 7.0 or higher
- MySQL/MariaDB
- Apache or similar web server

### Step 1: Create Database

```bash
mysql -u root -p < database.sql
```

When prompted, enter your MySQL password (default: `root`)

### Step 2: Start Local Server

```bash
cd /home/rakib/demo-php
php -S localhost:8000
```

### Step 3: Access Application

Open your browser and go to: `http://localhost:8000`

### Step 4: Test

1. Click "Register here" to go to register form
2. Enter username, email, password
3. Click Register
4. You will see success message
5. Login with your credentials
6. You will see dashboard

## Deployment to AWS EC2

### Prerequisites on EC2
- Ubuntu 20.04 or similar
- SSH access to EC2 instance

### Step 1: Connect to EC2

```bash
ssh -i your-key.pem ubuntu@your-ec2-public-ip
```

### Step 2: Install Required Packages

```bash
sudo apt update
sudo apt install apache2 php php-mysql mysql-server -y
```

### Step 3: Start Services

```bash
sudo systemctl start apache2
sudo systemctl start mysql
sudo systemctl enable apache2
sudo systemctl enable mysql
```

### Step 4: Create Database

```bash
sudo mysql -u root < database.sql
```

Or manually:

```bash
sudo mysql -u root
```

Then paste the content from `database.sql`

### Step 5: Upload Project Files

From your local machine:

```bash
scp -i your-key.pem -r /home/rakib/demo-php/* ubuntu@your-ec2-public-ip:/var/www/html/
```

### Step 6: Set Permissions

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

### Step 7: Update config.php for EC2

On EC2, edit `/var/www/html/config.php`:

```bash
sudo nano /var/www/html/config.php
```

Change database credentials if needed (default should work for self-contained setup)

### Step 8: Access Application

Open your browser and go to:
```
http://your-ec2-public-ip
```

## Database Details

**Database Name:** `login_system`

**Table:** `users`
- `id` - Auto-increment primary key
- `username` - Unique username (max 50 chars)
- `email` - Unique email (max 100 chars)
- `password` - Hashed password (bcrypt)
- `created_at` - Registration timestamp

## Security Notes

- Passwords are hashed using PHP's `password_hash()` with bcrypt
- SQL injection is prevented using prepared statements
- XSS is prevented using `htmlspecialchars()`
- Sessions are used to maintain login state

## Troubleshooting

### "Connection failed" error
- Check MySQL is running: `sudo systemctl status mysql`
- Verify database credentials in `config.php`

### "Permission denied" error on EC2
- Run: `sudo chown -R www-data:www-data /var/www/html`

### Database not found
- Run the database.sql script: `sudo mysql -u root < database.sql`
