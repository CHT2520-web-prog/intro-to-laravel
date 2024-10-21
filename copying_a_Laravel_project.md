# Copying Laravel Projects
You may want to copy a Laravel project you are working on:-
- To back-up your work.
- To move work from a USB stick to your home machine and vice versa. 

The best way to do this is to use Git and GitHub. We'll get onto using Git and GitHub soon, but for now you are probably still trying to get confident with Composer and Laravel. Here's how to make a copy of your work. 

- In your Laravel project, copy everything apart from the *vendor* folder.
> Do not copy the *vendor* folder. The vendor folder is where Composer installs the libraries. 
- On the machine you are copying to, create a new folder in *htdocs* for your project. 
- Paste the copied files into this folder
- Open the XAMPP shell/terminal in this folder.
- Do a quick check that you are in the correct directory
```
dir
```
- Make sure you can see the *composer.json* file listed. This is needed by Composer to install Laravel. 
- Enter the following Composer command:
```
composer install
```
- The project's dependencies should now download and install. 
- Remember to change the *DocumentRoot* seeting in your *httpd.conf* file to point to the *public* folder of your project. 