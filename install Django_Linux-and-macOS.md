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
source myenv/bin/activate
```
 >*NB*: Replace ***virtualenv myenv*** commande, with ***venv myenv*** if you install $\color{rgba(255,0,0, 0.4)}{\textsf{python3-venv}}$ package.<br>
 > - Replace path/to/your/project with the actual path to your project directory.<br>
 > - Once you've finished working on your Django project, you can deactivate the virtual environment:
 >> ```bash
 >> deactivate
 >> ```
> - *Remember to activate the virtual environment every time you work on your Django project and deactivate it when you're done. This practice will help you avoid conflicts between different projects and make your development workflow more efficient*.

 ## Step 4: Install Django
 
```bash
pip3 install django
```
Check the installed Django version to confirm a successful installation

```bash
python3 -m django --version
```
Save installed requirement list in text file

```bash
pip3 freeze
pip3 freeze > requirements.txt
```

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
Once the server is running you can view the site by navigating to the following URL on your local web browser: http://127.0.0.1:8000/. You should see a site that looks like this:

![Django development environment up and running](img/django_skeleton_app_homepage_django_4_0.png)

## Step 3: Create a Django App

Return to your terminal and stop the development server. In the terminal where the server is running press Ctrl+C.

Use the following command to create a Django app:

```bash
python3 manage.py startapp yourappname
```

>*NB: Replace yourappname with the desired name for your Django app. This command will generate a new directory with the specified app name, containing the necessary files and directories.

Once the app is created, navigate to the app directory to understand the *App Structure*.

```bash
cd yourappname
```

Here's a brief overview of the key files and directories within a Django app:

migrations/: Contains database migration files.

templates/: Optionally contains HTML templates for the app.

views.py: Defines the views (functions or classes) for the app.

models.py: Defines the data models for the app.

admin.py: Configures the app for the Django admin interface.

apps.py: Configures the app's metadata.

tests.py: Contains unit tests for the app.

init.py: An empty file that tells Python that the directory should be considered a Python package.

# Use the Django Admin Interface

## step 1: Creating a Superuser Account

You need to create a superuser account to access the Django Admin. to do that, follow the command:

```bash
python3 manage.py createsuperuser

```

Follow the prompts to enter a username, email, and password for the superuser account.


## step 2: Accessing the Admin Interfac

Now that you have a superuser account, start the development server:

````bash
python3 manage.py runserver
````

Open your web browser and go to [`https://localhost:8000/admin`]( http://127.0.0.1:8000/admin/). and Log in with the superuser credentials you created.


### Exploring the Django Admin Interface

Once logged in, you'll see the Django Admin Interface dashboard. Here are some key features:

- App List
The main dashboard displays a list of installed apps that have models registered with the admin interface.

- Model List
Click on an app to see a list of models registered for that app. For example, if you registered the MyModel in your admin.py, you'll see it listed.

- Adding New Entries
Click on a model to view and manage its entries. You can add new entries by clicking the "Add" button.

- Editing and Deleting Entries
Click on an existing entry to edit its details. You can also delete entries from the model list view.

- Search and Filters
Use the search bar and filters to quickly locate specific entries within a model.

## step 3: Using a Model Configuration
You can further configure how a model is displayed in the admin interface by creating a custom ModelAdmin class.
````bash
# myapp/admin.py

from django.contrib import admin
from .models import MyModel

class MyModelAdmin(admin.ModelAdmin):
    list_display = ('name', 'description')
    search_fields = ['name']

admin.site.register(MyModel, MyModelAdmin)
````

### Inline Models
You can display related models within the parent model's edit page using inline models.

````bash
# myapp/admin.py

from django.contrib import admin
from .models import ParentModel, ChildModel

class ChildModelInline(admin.TabularInline):
    model = ChildModel

class ParentModelAdmin(admin.ModelAdmin):
    inlines = [ChildModelInline]

admin.site.register(ParentModel, ParentModelAdmin)
````

## step 4: Customizing the Admin Interface
Django allows extensive customization of the admin interface, from customizing the look and feel to adding custom views. Here are some common customizations:

````bash
# myapp/admin.py

from django.contrib import admin
from django.urls import path
from .views import custom_admin_view

admin.site.index_template = 'admin/myapp/index.html'

admin.site.app_index_template = 'admin/myapp/app_index.html'

admin.site.site_header = 'My Custom Admin'

admin.site.site_title = 'My Custom Admin'

admin.site.register_view('custom-view/', view=custom_admin_view, name='custom_view')
````

- Customizing the Admin Site Header
Override the admin/base_site.html template to customize the site header.

- Adding Custom Views
Create custom views and add them to the admin interface.

- Theming
Apply custom styles to the admin interface by overriding the default styles.

- Customizing List Displays
Define the list_display attribute in your ModelAdmin class to customize the columns displayed in the list view.

- Overriding Templates
Override default templates to customize the appearance of specific admin views.


