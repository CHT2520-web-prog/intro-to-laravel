# # Installing Laravel

The official installation instructions for laravel are very good - https://laravel.com/, but they don't explain how to get up and running if you are using XAMPP on a USB stick. 

## Installing Composer

First we need to download composer. Composer is a dependency manager. We use composer to load and manage third-party code and libraries. For us this means downloading and managing Laravel and all the libraries that Laravel depends on. 

Click on the following link.

```
https://getcomposer.org/composer.phar
```

- Save the file into the PHP folder of XAMPP
- From the XAMPP control panel click 'shell'
- A command line shell should open. Enter:

```
cd php
```

Then enter

```
dir
```

The list of all the files and folders in the php folder will be shown. Make sure you can see the _composer.phar_ file you have just downloaded.

- To make Composer accessible we need to create a .bat file. Copy the following carefully into the XAMPP shell (this generates a batch file)

```
echo @php "%~dp0composer.phar" %*>composer.bat
```

Now enter

```
composer
```

If it has installed correctly, a list of composer commands will be shown

- You can now close the shell window

## Installing a Laravel Project

- Open the XAMPP shell
- Change directory to the htdocs folder

```
cd htdocs
```

- To install laravel enter the following composer command (this will create a Laravel project called film-app)

```
composer create-project laravel/laravel film-app
```

- Composer should now download Laravel and download Laravel's dependencies (this may take a bit of time).

> What if I get a timeout error? Occassionally I have experienced Composer timing out when setting up a Laravel project. If this happens, change the timeout duration by entering the following 
```composer config --global process-timeout 2000```. Then try and create your project again. 

## Changing the DocumentRoot

- We will change the Apache server settings so that web directory route is the Laravel folder.
- From the control panel, next Apache, select config. This will open a file called _httpd.conf_.
- In _httpd.conf_ we want to change the DocumentRoot setting. This should be at about line 250 and will look like:-

```
DocumentRoot "/xampp/htdocs"
<Directory "/xampp/htdocs">
```

This specifes the DocumentRoot is the _htdocs_ folder. When the user enters http://localhost Apache runs pages in the _htdocs_ folder.
Change this to:-

```
DocumentRoot "/xampp/htdocs/film-app/public"
<Directory "/xampp/htdocs/film-app/public">
```

This changes the DocumentRoot to the public folder in Laravel. Now when the user enters http://localhost Apache will run _index.php_ in the this folder.

- Save this file.
- Restart Apache.
- Open a browser.
- Enter http://localhost and you should see the default laravel page. This means you have successfully created a Laravel project. 
