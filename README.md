
E-Commerce_LAMP_Stack

To configure and deploy the LAMP stack project from the given GitHub repository, follow these steps:

**1. Prerequisites**
Ensure the following are installed on your system or server:
**Apache:** To serve the web application.
**MySQL:** To handle the database.
**PHP:** For dynamic web content.
**Git:** To clone the repository.
**Linux-based OS:** Preferably Ubuntu or CentOS.

**2. Install and Set Up LAMP Stack**
For Ubuntu:

# Update packages
sudo apt update && sudo apt upgrade -y

# Install Apache
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2

# Install MySQL
sudo apt install mysql-server -y

# Install PHP and required extensions
sudo apt install php libapache2-mod-php php-mysql php-cli php-curl php-xml php-mbstring -y

**3. Clone the GitHub Repository**
# Navigate to the web server directory
cd /var/www/html

# Clone the repository
sudo git clone https://github.com/Riadz/E-commerce.git

# Set proper permissions
sudo chown -R www-data:www-data /var/www/html/E-commerce
sudo chmod -R 755 /var/www/html/E-commerce

**4. Configure Apache**
Create a virtual host configuration file for the project:
sudo nano /etc/apache2/sites-available/ecommerce.conf

Add the following configuration:

<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/E-commerce
    ServerName ecommerce.local

    <Directory /var/www/html/E-commerce>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/ecommerce_error.log
    CustomLog ${APACHE_LOG_DIR}/ecommerce_access.log combined
</VirtualHost>


Enable the site and rewrite module:
sudo a2ensite ecommerce.conf
sudo a2enmod rewrite
sudo systemctl restart apache2

Update your /etc/hosts file to map the domain to your local server:
sudo nano /etc/hosts
Add:
127.0.0.1 ecommerce.local

**5. Configure the MySQL Database**
Secure the MySQL installation (if not done):
sudo mysql_secure_installation

#Log in to MySQL:
sudo mysql -u root -p

#Create a database and user for the project:
CREATE DATABASE ecommerce;
CREATE USER 'ecommerce_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON ecommerce.* TO 'ecommerce_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;

Import the database if the repository includes a .sql file:
sudo mysql -u ecommerce_user -p ecommerce < /var/www/html/E-commerce/_database/e_commerce.sql

**6. Configure the Application**
Update configuration files (if applicable) for database connection in PHP:
Look for a file like config.php, .env, or similar.
Update database credentials:
define('DB_HOST', 'localhost');
define('DB_NAME', 'ecommerce');
define('DB_USER', 'ecommerce_user');
define('DB_PASSWORD', 'your_password');


**7. Test the Application**
Open a browser and navigate to:
http://ecommerce.local
Check if the application is running correctly.
