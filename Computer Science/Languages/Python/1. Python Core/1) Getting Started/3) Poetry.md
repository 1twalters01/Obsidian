# About Poetry
* Poetry is a python package manager
* Poetry uses a `pyproject.toml` file to manage dependencies, which is more modern and easier to read than Pip's requirements.txt file.
* The rest of this guide assumes the use of Poetry.
* To work on existing projects all you have to do is run:
	* `git clone example.org/someproject`
	* `cd some project`
	* `poetry install`

## Installation
1) Install poetry
	* Linux, macOS: enter `curl -sSL https://install.python-poetry.org | python3 -` into a terminal
	* On windows cmd: type `Powershell` and then type `(Invoke-WebRequest -Uri https://install.python-poetry.org -UseBasicParsing).Content | py -`
2) Add poetry to your paths:
	* Linux/Unix: `~/Library/Application Support/pypoetry`
	* MacOS: `~/.local/share/pypoetry`
	* Windows: ``%APPDATA%\pypoetry`
3) Check that poetry has installed correctly by typing `poetry --version`
4) Run the command `poetry config virtualenvs.in-project true` to put the venv inside the project folder

## Uninstallation
In the event of wanting to uninstall poetry, enter the following:

```
curl -sSL https://install.python-poetry.org | python3 - --preview
curl -sSL https://install.python-poetry.org | POETRY_PREVIEW=1 python3 -
```

## Creating a project with Poetry
Run the following on any operating system to create a project with Poetry:
`cargo new file_name`
`cd file_name`

The first command creates a new directory and project called `file_name`.

Open `Cargo.toml`. You should see the following:
```toml
[tool.poetry]
name = "file_name"
version = "0.1.0"
description = ""
authors = ["1twalters01 <1twalters01@gmail.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.11"


[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api
```

This file is in the TOML format, which is Cargo's configuration format.
* The variables under `[tool.poetry]` are about your project
* The variables under `[tool.poetry.depencencies]` show the dependencies your project has
	* The `^` symbol for ^3.11 means any version greater than or equal to 3.11.0 and less than 3.12.0
* The variables under `[build-system]` are the requirements for poetry to work

### Installing dependencies
In the terminal, navigate to the project. Add new dependencies by typing: `poetry add dependency`.

Let's add NumPy, a scientific computation library to our project:
`poetry add numpy`

### Improving upon the project
In the `file_name` directory, create the following files: `__main__.py` and `app.py`.

In the `__main__.py` file, write the following:
```python
from .app import start

def app():
    start()
```

In the `app.py` file, write the following:
```python
import numpy as np

def start():
    a = np.arange(15).reshape(3,5)
    print(a)
```

Add the following to the `pyproject.toml`:
```toml
[tool.poetry.scripts]
script = "file_name.__main__:app"
```

The `pyproject.toml` file should thus look like:
```toml
[tool.poetry]
name = "file_name"
version = "0.1.0"
description = ""
authors = ["1twalters01 <1twalters01@gmail.com>"]
readme = "README.md"

[tool.poetry.scripts]
script = "file_name.__main__:app"

[tool.poetry.dependencies]
python = "^3.11"
numpy = "^1.25.1"


[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

To run the project, go to the main project directory and type: `poetry run script`

