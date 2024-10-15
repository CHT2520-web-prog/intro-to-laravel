# Getting Started with Laravel

There are lots of different ways of doing this. 
- XAMPP on a USB stick.
- XAMPP on a Windows PC or Mac.
- Laravel Herd on Windows PC or Mac.

I haven't used it, but I know several people have found Laravel Herd (https://herd.laravel.com/) to be really good. This might be the easiest way to get up and running on your home machine. 

The following instructions focus on using XAMPP.

## Using XAMPP
You will already have PHP and a web server (Apache) installed. The final thing you need is Composer. 

> Composer is a dependency manager. Composer is used to load and manage third-party code and libraries. For us this means downloading and managing Laravel and all the libraries that Laravel depends on. 

## If you are using XAMPP on a USB Stick
- Click on the following link https://getcomposer.org/composer.phar

- Save the file into the PHP folder of XAMPP
- From the XAMPP control panel click 'shell'
- A command line shell should open. Enter:

```
cd php
```

* Then enter

```
dir
```

- The list of all the files and folders in the php folder will be shown. Make sure you can see the _composer.phar_ file you have just downloaded.
- To make Composer usable we need to create a *.bat* file. Copy the following carefully into the XAMPP shell (this generates a batch file)

```
echo @php "%~dp0composer.phar" %*>composer.bat
```

Now enter

```
composer
```

If it has installed correctly, a list of composer commands will be shown

- You can now close the shell window

## If you are using XAMPP on a windows PC or Laptop
- From the Composer homepage (https://getcomposer.org/download/) select Download
- Download and run *Composer-Setup.exe* - it will install the latest Composer version.
    - Check the PHP location is correct (it should point to the *php.exe* on your xampp e.g. *C:\xampp\php\php.exe*). Check the box that says add php to your path.
    - You shouldn't need to use a proxy so you can just say next
    - Then hit install
Testing it works
- In File Explorer open your *xampp/htdocs* folder
- Right-click and select 'Open in Terminal'. A terminal window should open. 
- Enter ```composer```.
- If it has installed correctly, a list of composer commands will be shown.

## If you are using XAMPP on a Mac
- I can't test this, but there are instructions for installing on a Mac: https://getcomposer.org/doc/00-intro.md#installation-linux-unix-macos 
- Follow these instructions to install Composer.