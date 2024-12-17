# install Django 
## Step 1: Install Python using Homebrew (Optional but Recommended)

If you prefer using Homebrew to manage Python, you can install or upgrade Python with the following command:

```bash
brew install python
```
This ensures you have an up-to-date Python version.

## Step 2: Install Virtual Environment(Recommended)

Using a virtual environment is a good practice to keep your project dependencies isolated. Install the virtualenv package with the following command:

```bash
pipx install virtualenv
```
OR
```bash
sudo apt install python3-venv
```

## Step 3: Create a Virtual Environment

```bash
cd path/to/your/project
virtualenv myenv
source venv/bin/activate
```
 >*NB*: Replace ***virtualenv myenv*** commande, with ***venv myenv*** if you install  $${\color{red}python3-venv \space}$$ package.<br>
 >Replace path/to/your/project with the actual path to your project directory.

 ## Step 4: Install Django
 
```bash
pip3 install django
```
Check the installed Django version to confirm a successful installation

```bash
python3 -m django --version
```
Once the server is running you can view the site by navigating to the following URL on your local web browser: http://127.0.0.1:8000/. You should see a site that looks like this:

# Setup Project

## Step 1: Create a Django Project

Create a new Django project with the following command:

```bash
django-admin startproject yourprojectname
```
and navigate to the Project Directory

```bash
cd yourprojectname
```
>*NB: Replace yourprojectname with the desired name for your Django project.

## Step 2: Run the Development Server

Ensure everything is set up correctly by starting the development server:

```bash
python manage.py runserver
```

Visit http://127.0.0.1:8000/ in your web browser to confirm the successful installation.

## Step 3: Create a Django App

return to your bash and stop the server with ctl-c

Use the following command to create a Django app:

```bash
python3 manage.py startapp yourappname
```
Once the app is created, navigate to the app directory 

>*NB: Replace yourappname with the desired name for your Django app. This command will generate a new directory with the specified app name, containing the necessary files and directories.
