# Python Virtual Environment and pip Cheatsheet

## Virtual Environment Introduction
Python has many external libraries. To use those external libraries, some dependencies need to be installed. To preserve the exact version of those dependencies while sharing the project, we use virtual environments. It isolates the dependencies to only the project

### Create a virtual environment
Run this command in the project root
```bash
python3 -m venv .venv
```
You will see a .venv directory has been created in the project root.

### Activate virtual environment
```bash
source .venv/bin.activate
```
When venv is activated, it will show something like this
```bash
(.venv) ssaaaadd@fedora:~/MbBootcamp/python-data-projects/ipl-analysis$ which python3
~/MbBootcamp/python-data-projects/ipl-analysis/.venv/bin/python3
```

### Deactivate virtual environment
```bash
deactivate
```

## pip Introduction:
pip is used to install the external dependencies in python. External dependencies can be used to extend python's functionality

### Install dependency
```bash
pip install matplotlib
```

### See dependency list
```bash
pip list
```

### Export dependencies
```bash
pip freeze > requirements.txt
```

### Install project depenedencies
```bash
pip install -r requirements.txt
```
