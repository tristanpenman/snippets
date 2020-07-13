# Simple setup for WordPress

I can never quite remember the syntax for creating a fresh database and user when messing around with WordPress locally:

    CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
    CREATE USER 'wordpress'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
    GRANT ALL ON wordpress.* TO 'wordpress'@'%';
    FLUSH PRIVILEGES;
