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
 >*NB*: replace ***virtualenv myenv*** commande, with ***venv myenv*** if you install  $${\color{red}python3-venv \space}$$ package.

 
