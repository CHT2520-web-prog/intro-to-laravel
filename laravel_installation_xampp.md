Getting Started with Laravel on your Home Machine

There are several options here. 

I haven't used it, but I know several people have found Laravel Herd to be really good. This might be the easiest way to get up and running. 

Using XAMPP
(This assumes Windows, there are Mac OS instructions - https://getcomposer.org/doc/00-intro.md#installation-linux-unix-macos)
In order to install Laravel we are going to need Composer
From the Composer homepage (https://getcomposer.org/download/) select Download
Download and run Composer-Setup.exe - it will install the latest composer version whenever it is executed.
Check the PHP location is correct (it should point to the php.exe on your xampp e.g. C:\xampp\php\php.exe)
Check the box that says add php to your path
You shouldn't need to use a proxy so you can just say next
Then hit install
Composer should be installed

Testing it works
In the file explore open your xampp htdocs folder
right click and select open in terminal/powershell
check composer is working
Enter the following composer command
composer create-project laravel/laravel film-app
