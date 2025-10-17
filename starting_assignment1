# Assignment Starter

## Step 1 - Create an assignment repository

-   Look for the green 'Use this template' button in the top-left of this page and select 'Create a new repository'. You should be taken to a new page.
    -   Enter a name for your repository e.g. _assignment1_
    -   Under 'Choose visibility', make the repository private. It's important to make the repo private. You don't want someone else copying your work!
    -   Finally select 'Create repository'. A repository will be created for you.

## Step 2 - Create a codespace from your repository

-   Open a new tab
-   Visit https://github.com/codespaces
-   Select 'New codespace'. You should be taken to a new page.
-   From the 'Select a repository' dropdown select the name of the repository you have just created.
-   Select 'Create codespace'. A codespace containing the repository code will be created for you.

## Step 3 - Install dependencies

The codespace already has Laravel installed. You just need to complete setting up and configuring Laravel before you can start work on the assignment.

-   In your codespace, in the terminal enter:

```
composer install
```

-   This will install all the libraries needed for Laravel to work.

## Step 4 - Configure the application using a .env file

-   Create a new file name _.env_ in the root of the codespace.
-   Copy the entire contents of _example.env_ into _.env_
-   In _.env_
    -   Change the `APP_URL` to be the following e.g.
    ```
    APP_URL=https://name-of-codespace-8000.app.github.dev/
    ```
    -   You can get the 'name-of-codespace' from the URL in the browser. It will be two words and a random series of letters and numbers e.g. _special-potato-ppvp74vgxfrg6r_. So for this example I'd enter an APP_URL of:-
    ```
    APP_URL=https://special-potato-ppvp74vgxfrg6r-8000.app.github.dev/
    ```
    -   Change the database settings to the following:
    ```
    DB_CONNECTION=mysql
    DB_HOST=db
    DB_PORT=3306
    DB_DATABASE=cht2520
    DB_USERNAME=root
    DB_PASSWORD=secret
    ```
-   Save your changes to `.env`
-   In the terminal enter the following:

```
php artisan key:generate
```

This will generate a key that is used to encode potentially sensitive data e.g. session data, cookies etc.

-   Then enter

```
php artisan migrate
```

This will generate some basic database tables that Laravel needs to work.

## Step 5 - Test the installation is successful

In the terminal enter

```
php artisan serve
```

You should be able to view you Laravel app on port 8000 where you will see the 'Let's get started' message.

## Step 6 - Using Git

-   Back in your Codespace, open the README.md file.
-   Delete the entire contents and enter the following:

```
# CHT2520 Assignment 1 U0123456 Firstname Lastname
```

-   Save the changes.

-   From the menu on the lefthand side, select Source Control (it looks like some circles connected by lines)

-   Enter a message e.g. 'Initial commit'

-   Select 'Commit'
-   Answer 'yes' to staging all changes
-   Now click 'Sync Changes'
-   You'll be asked if you want to pull and push changes
-   Answer 'ok'
-   Go back to the tab contained your assignment1 repository in Github. Refresh this page and check you can see the updated README.

## Step 7 - Get on with the assignment

Now you are set-up, you won't need to go through this process again.
You simply need to develop your app in the codespace and make sure you regularly push your changes to your assignment repo.
