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