#Modules, packages and friends
##Modules
Recall that any .py file is a module. A good module is one that has a suitable name, docstring and is relevant to one subject. To use a module elsewhere in code, we import it. The import operation allows us to reuse the code written in the imported module. When we import a module, the Python interpreter will begin by searching for it under previously imported modules, built in modules and, ultimately, path based modules. The module we'll be using will be found by the path based import.

>Create a program that prints to screen whatever is given to it through the CLI. Split it into two modules. One will parse the CLI arguments and the other will perform the printing. If no argument is given, it should print "Modules modules modules".

##Packages
If a module is a .py file then a package is a directory containing one or more modules and zero or more subpackages (those subpackages are packages on their own). Once a project grows to a large scale, it becomes easier to maintain and develop using several modules instead of one large module (.py file). When this happens, and it is comprised of several modules, the project can be referred to as a package. A large project can be split into multiple smaller packages/projects that, in turn, can each be useful on their own. But, that is a versatility that we won't use, for now. Just know that it exists. To enable traversal to the package directory when importing from it, it must contain an `__init__.py` file. 

>Create a `__init__.py` file in the root of your project. Inside, write a short docstring that introduces your program to the world and include a simple usage example with input and output.

>Go to the parent directory and import one of your modules using `from my_project import my_module` through the Python interpreter.


When a package is installed, the resources it exposes in its `__init__.py` will be the ones the user will have easy access to. What this means is that we have to import our useful function[s]/class[es] or module[s] in our `__init__.py`. Before we do this with our `__init__.py` file, we'll touch upon one more subject - relative imports. To eliminate the chance for ambiguity and errors by importing the wrong module, we'll use `from .my_proect import my_module`. This type of import, prefixing the package/module name with a dot, is a relateive import. It will import the module found in its directory location. In other words, writing such an import in our `__init__.py` will always import our work.

>Import your function that does the bulk of the work in your module within `__init__.py`. 
>Test your `__init__.py` file by importing your package while outside the root of it and using your exposed function.

##Pip Installs Packages
The standard method to install packages in a Python environment is by using `pip`. When we install things manually or, worse, just dump a package and import from it, we risk breaking things. Either the package itself by not providing it with its dependencies or our own project when managing our library usages quickly gets out of hand. Instead, `pip` installs packages gracefully by, most importantly, handling dependencies and their versions for us. By using `pip`, we can also avoid literring the system Python installation with various packages with the `--user` flag (most relevant if we're using Linux). But, as we're using `venv` to create isolated Python environments, we don't need to concern ourselves with that.


Installing packages with pip is as straight-forward as
```
pip install <package_name>
```
We can uninstall packages with `pip uninstall` and search for them using `pip search`. The rest of the options are available through `pip help`.

>Install the `requests` package to your `venv` environment. Use `which pip` to ensure you're using your isolated environment's `pip`.

##Using setuptools for distribution
The distribution of our work is basically a packed version of our project. This packed version can, later, be used with `pip` to install  into a new Python enviornment. Using this method, we can safely install our package elsewhere or, in other words, port it. The distribution setup information includes some key information such as dependencies, its name, version, author and license. The setuptools utility helps us create such a distribution.


Before we can distribute our project using setuptools, though, we have to make sure our directory structure looks a certain way. Afterwards, we'll tell setuptools where our project is and what to include in the distribution. A good starting point should look like this
```
project_name/
|
+--LICENSE
|
+--setup.py
|
+--README.rst
|
+--project_name/
   |
   +--__init__.py
   |
   +--some_module.py
```

Like any good software, our project must include a README file. Within, we should write a helpful description of what our tool does and how to use it. Examples, details and tips are what make a good README. The .rst format, reStructuredText, that the README is in, lets us structure the text with simple formatting such as titles and code highlighting.

>Create a README.rst file with the docstring of your program above.

You can augment your .rst file using `Title\n====` to create titles and `.. code-block:: python` to begin highlighting Python code. You can see a live example here `https://github.com/requests/requests/blob/master/README.rst` (use the `raw` button to see the code).

Good old fashioned manual can be found here `http://docutils.sourceforge.net/rst.html`.

>Use a title and code highlightning in your README.rst.


The `setup.py` file instructs setuptools what our distribution will include. Our distribution should include our package (and its modules), its name, its author, license, dependencies and extra instructions as needed. 
A basic `setup.py` file will look like this
```
from setuptools import setup, find_packages
setup(
    name="project_name",
    version="0.1",
    packages=find_packages(),
    # If we want to provde an executable to our package
    entry_points={
        'console_scripts': [
            'executable_name = project_name.module:function_name'
        ]
    },
    package_data={
        # If any package contains *.rst files, include them:
        '': ['*.rst']
    },

    # Information that'll be displayed on PyPI
    author="Me",
    author_email="me@mail.com",
    description="A brief description",
    license="MIT",
    keywords="network security awesome"
)
```

Most keywords are self explanatory such as name, version, author, author_email, description and license. These describe the respective attributes of our package. 


`packages` defines what packages the distribution will include. The helper function `find_packages` from setuptools will look for directories in the script's location and include them as packages. This eases our maintenance of `setup.py` as we change our package through time. 


`entry_points` defines what executables to install. This is completely optional and creates an executable in the environment's bin/ that starts a function of our choice. In the above example an executable named `executable_name` will be available straight from the terminal and will start the function `function_name` under the aptly named module `module`.


`package_data` includes any extra files as part of the distribution to be added to the pack. This is useful for our README file as it isn't a module and won't be included in the distribution by default.


`keywords` helps tools such as PyPI yield our package during search.

>Create your own `setup.py` using the above template. 

For more details about setuptools, see `https://setuptools.readthedocs.io/en/latest/setuptools.html`.

##Installing and distributing
While we develop, we don't want to constantly install/uninstall our distribution after every change we make to our code. Instead, we want to see our changes take effect immediately, for testing purposes. To avoid this back and forth game, `pip` gives us the option to install a package in development mode. This allows us to continue coding without reinstalling our package each time we make a change. Using development mode only creates a link to our package so every update we do to the code is immediately reflected in use.
This is, also, a good way to test our `setup.py`.


To install our distribution in development mode, change directory to the distribution's root directory and use
```
pip install -e .
``` 
What we're doing here is asking `pip` to install under development mode using `-e` and indicating the package distribution to install is under the current directory, noted with a dot.
>Install your package in development mode.
>Ensure your package installation worked by using your module through the Python interpreter from a different directory.


Publishing to PyPI requires we register on PyPI's site, create a source distribution and then upload it using our credentials. 
Start off by installing `twine` using `pip`. Twine will let us securely communicate with PyPI using HTTPS, hiding our credentials from plain sight. Setuptools can also upload to PyPI but, it uses HTTP.
Create your source distribution folder using `python setup.py sdist`. This will create a `dist` folder with our distribution which we'll soon upload.
We'll be using test.pypi.org (which purges every so often) to test publishing to PyPI and avoid affecting the real index. Register at https://test.pypi.org/account/register/.
While in your package directory, upload your dist using
```
twine upload --repository-url https://test.pypi.org/legacy/ dist/*
```
The parameter `--repository-url https://test.pypi.org/legacy/` tells twine to upload to an alternate site, the PyPI testing address. Without this parameter, it will upload to the conventional PyPI. The project will be avaialble at https://test.pypi.org/project/<project_name>


Now, you can install your package using
```
pip install --index-url https://test.pypi.org/simple/ your-package
```
Here, we instruct pip to use a different site rather than the default PyPI.
You can uninstall your package (even in development mode) using
```
pip uninstall package_name
```


Once you work on your own project and wish to upload it to PyPI, remember to remove the testing URL from `twine` and `pip`. Both will default to work with pypi.org.

##PyInstaller
Pyinstaller allows us to deploy standalone versions of our package. We can transfer it between machines and not have to worry about an installation process (such as `pip`). The only requirement is that the OS and architecture match the one it was prepared on. If we want to run it on a different OS or architecture, we'll have to prepare our standalone program, using pyinstaller, on that combination of OS and architecture. Most Windows flavors are interchangable but assert your deployment with a test.


We'll start by installing `pyinstaller` using pip. Afterwards, if we want to turn our "main" module to an executable, we'll run the following
```
pyinstaller --onefile --noconfirm path/to/our_module.py
```
What this doesn is create an executable equivalent to `python our_module.py` which is standalone and doesn't require a present Python installation. The executable will be under dist/. PyInstaller also creates a build/ folder for its build files but we don't need them.

>Augment your module to run when started from the terminal and then create a standalone executable of it.