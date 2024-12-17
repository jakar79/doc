# install Django on macOS
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

## Step 3: Create a Virtual Environment##

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
## Step 5: Create a Django Project
Create a new Django project with the following command:
```bash
django-admin startproject yourprojectname
```
and navigate to the Project Directory

```bash
cd yourprojectname
```
>*NB: Replace yourprojectname with the desired name for your Django project.

Step 6: Run the Development Server
Ensure everything is set up correctly by starting the development server:
```bash
python manage.py runserver
```

Visit http://127.0.0.1:8000/ in your web browser to confirm the successful installation.

Congratulations! You've now successfully installed Django on your macOS. You're ready to dive into the world of Django and start building your web applications with this robust and feature-rich framework.
