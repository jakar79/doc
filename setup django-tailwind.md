# Setup django-tailwind

## Requirements


   Before we install Django we will get you to install an extremely useful tool to help keep your coding environment tidy on your computer. It's possible to skip this step, but it's highly recommended. Starting with the best possible setup will save you a lot of trouble in the future!

   So, let's create a virtual environment (also called a venv or virtualenv). venv (Virtualenv) will isolate your Python/Django setup on a per-project basis. This means that any changes you make to one website won't affect any others you're also developing.

   All you need to do is find a directory in which you want to create the virtual envirenment, your Development directory, it might look like `C:\User-Name\devdir\` On Windows, or `/home/user-name/devdir/` On Linux

   >*NB*: On Windows, make sure that this directory does not contain accented or special characters; if your username contains accented characters, use a different directory, for example, `C:\djangoapp`.


   For this tutorial we will be using a new directory `djangoapp` from your home directory:

   ```bash
      mkdir djangoapp
      cd djangoapp
   ```

   We will make a virtualenv called myvenv. The general command will be in the format:

   ```bash
      python3 -m venv myvenv
   ```

>*NB*: On some versions of Debian/Ubuntu you may receive the following error:
>
>
>  The virtual environment was not created successfully because ensurepip is not available.  On Debian/Ubuntu systems, you need to install the python3-venv package using the following command.
>>     sudo apt install python3-venv
>  *After installing the python3-venv package, recreate your virtual environment*.

1. Start your virtual environment by running:

   ```bash
    source myvenv/bin/activate
   ```

2. you’ll need to install Django on your local computer by running the following command in the terminal if you have pip available in your Python environment:

   ```bash
    python3 -m pip install Django
   ```
   >*NB*: Before we do that, we should make sure we have the latest version of pip, the software that we use to install Django:
   >>   `python -m pip install --upgrade pip`

3. Run the following command in the terminal to create a new Django project with the name myapp:

   ```bash
    django-admin startproject myapp
    cd myapp/
   ```
 
## Step-by-step installation & configuration instructions of `Django-Tailwind` 

1. Install the `django-tailwind` package via `pip`:

   ```bash
   python3 -m pip install django-tailwind
   ```

   If you want to use automatic page reloads during development (see steps 10-12 below)
   use the `[reload]` extras, which installs the `django-browser-reload` package
   in addition:

   ```bash
   python3 -m pip install 'django-tailwind[reload]'
   ```

   Alternatively, you can install the latest development version via:

   ```bash
   python3 -m pip install git+https://github.com/timonweb/django-tailwind.git
   ```

2. Add `'tailwind'` to `INSTALLED_APPS` in `settings.py`:
   ```python
   INSTALLED_APPS = [
     # other Django apps
     'tailwind',
   ]
   ```

3. Create a new templates/ directory inside the project folder and update settings.py folder:
   ```python
   
        TEMPLATES = [
                {
                    ...
                    'DIRS': [BASE_DIR / 'templates'], # new
                    ...
                },
            ]
   
   ```

4. Create a *Tailwind CSS* compatible *Django* app, I like to call it `theme`:

   ```bash
   python3 manage.py tailwind init
   ```

5. Add your newly created `'theme'` app to `INSTALLED_APPS` in `settings.py`:

   ```python
   INSTALLED_APPS = [
     # other Django apps
     'tailwind',
     'theme'
   ]
   ```

6. Register the generated `'theme'` app by adding the following line to `settings.py` file:

   ```python
   TAILWIND_APP_NAME = 'theme'
   ```

7. Make sure that the `INTERNAL_IPS` list is present in the `settings.py` file and contains the `127.0.0.1` ip address:

   ```python
   INTERNAL_IPS = [
       "127.0.0.1",
   ]
   ```

8. Install *Tailwind CSS* dependencies, by running the following command:

   ```bash
   python3 manage.py tailwind install
   ```

9. The *Django Tailwind* comes with a simple `base.html` template located at
   `your_tailwind_app_name/templates/base.html`. You can always extend or delete it if you already have a layout.

10. If you are not using the `base.html` template that comes with *Django Tailwind*, add `{% tailwind_css %}` to
           the `base.html` template:
        
           ```html
           {% load static tailwind_tags %}
           ...
           <head>
              ...
              {% tailwind_css %}
              ...
           </head>
           ```

    The `{% tailwind_css %}` tag includes Tailwind's stylesheet.


11. Let's also add and configure `django_browser_reload`, which takes care of automatic page and css refreshes in the
    development mode. Add it to `INSTALLED_APPS` in `settings.py`:

    ```python
    INSTALLED_APPS = [
      # other Django apps
      'tailwind',
      'theme',
      'django_browser_reload'
    ]
    ```

12. Staying in `settings.py`, add the middleware:

    ```python
    MIDDLEWARE = [
      # ...
      "django_browser_reload.middleware.BrowserReloadMiddleware",
      # ...
    ]
    ```

    The middleware should be listed after any that encode the response, such as Django’s `GZipMiddleware`. The middleware
    automatically inserts the required script tag on HTML responses before `</body>` when `DEBUG` is `True.`

13. Include `django_browser_reload` URL in your root `url.py`:

       ```python
       from django.urls import include, path
       urlpatterns = [
           ...,
           path("__reload__/", include("django_browser_reload.urls")),
       ]
       ```

14. Finally, you should be able to use *Tailwind CSS* classes in HTML. Start the development server by running the
    following command in your terminal:

      ```bash
      python3 manage.py tailwind start
      ```

    
    >*NB*: To create a production build of your theme, run: `python3 manage.py tailwind build`
    >This will replace the development build with a bundle optimized for production. No further actions are necessary; you can deploy!

## Optional configurations

### Content (formerly Purge) rules configuration

The `content` section of your `tailwind.config.js` file is where you configure the paths to all of your HTML templates,
JavaScript components, and any other source files that contain *Tailwind* class names.

Depending on your project structure, you might need to configure the `content` rules in `tailwind.config.js`. This file
is in the `static_src` folder of the theme app created by `python manage.py tailwind init {APP_NAME}`.

For example, your `theme/static_src/tailwind.config.js` file might look like this:

```js
module.exports = {
    content: [
        // Templates within theme app (e.g. base.html)
        '../templates/**/*.html',
        // Templates in other apps
        '../../templates/**/*.html',
        // Ignore files in node_modules
        '!../../**/node_modules',
        // Include JavaScript files that might contain Tailwind CSS classes
        '../../**/*.js',
        // Include Python files that might contain Tailwind CSS classes
        '../../**/*.py'
    ],
    ...
}
```

Note that you may need to adjust those paths to suit your specific project layout. It is crucial to make sure that *all*
HTML files (or files containing HTML content, such as `.vue` or `.jsx` files) are covered by the `content` rule.

For more information about setting `content`, check out the *"Content Configuration"* page of the Tailwind CSS
docs: [https://tailwindcss.com/docs/content-configuration](https://tailwindcss.com/docs/content-configuration).

### Configuration of the path to the `npm` executable

*Tailwind CSS* requires *Node.js* to be installed on your machine.
*Node.js* is a *JavaScript* runtime that allows you to run *JavaScript* code outside the browser. Most (if not all) of
the current frontend tools depend on *Node.js*.

If you don't have *Node.js* installed on your machine, please follow installation instructions
from [the official Node.js page](https://nodejs.org/).

Sometimes (especially on *Windows*), the *Python* executable cannot find the `npm` binary installed on your system. In
this case, you need to set the path to the `npm` executable in *settings.py* file manually (*Linux/Mac*):

```python
NPM_BIN_PATH = '/usr/local/bin/npm'
```

On *Windows*, you may have npm on `$PATH` but it's `npm.cmd` rather than `npm`. (You can call it from the terminal because `$PATHEXT` contains `.cmd`.) If so, please override the default `NPM_BIN_PATH = 'npm'`:

```python
NPM_BIN_PATH = 'npm.cmd'
```

Alternatively (and for maximum reliability), you can use a fully qualified path. It might look like this:

   ```python
   NPM_BIN_PATH = r"C:\Program Files\nodejs\npm.cmd"
   ```
   
   >*NB*: that the path to the `npm` executable may be different for your system. To get the `npm` path, try running
   the command `which npm` in your terminal. (On *Windows*, please try `where npm` or `Get-Command npm`)
   >
   >If you share codes with others, you can search `$PATH` (and `$PATHEXT` on Windows) dynamically in *settings.py*:
   
   ```python
   from shutil import which
   NPM_BIN_PATH = which("npm")
   ```

> *source : https://django-tailwind.readthedocs.io/en/latest/index.html*
