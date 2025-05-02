# UV: Unified Python Package & Project Management

**UV** is a tool designed to streamline **package** and **project** management in Python, simplifying the complexities around package managers like `pip` and `conda`. It integrates Python version management and dependency management into a unified tool, making workflows smoother and more efficient.

## Motivation

In the Python ecosystem, multiple tools like `pip`, `conda`, and `poetry` overlap in their responsibilities, leading to fragmentation. Different tools work with different configuration files (e.g., *requirements.txt* for pip, *env.yml* for conda), and package authors may only support certain tools. To mitigate this, several PEP proposals later, the *pyproject.toml* has become the new standard. **UV** aims to leverage the *pyproject.toml* file to ease developer challenges by providing a single tool for both project and package management.

This README will guide you through the most common workflows in **UV**, how to think about UV, and how to use it to replace your current pip/conda setup.

## Installation

To install **UV**, follow the instructions on the [official installation page](https://docs.astral.sh/uv/getting-started/installation/).

> **Note:** The examples here are from the official docs and provided for quick reference.

---

## Package Management Commands

These commands help you initialize and manage your Python projects, install/remove dependencies, and handle environment configuration.

### `uv init`
Initialize a new project in the working directory, or in a target directory by providing a name (e.g., `uv init new_project`). If you have an existing project, use `--bare` argument.

### `uv add / remove`

- `uv add httpx`  
  Install the package *httpx*.

- `uv add "httpx>=0.20"`  
  Install with a version constraint (`>= 0.20`).

- `uv add "httpx @ git+https://github.com/encode/httpx"`  
  Install a package directly from a Git repository.

- `uv remove httpx`  
  Remove the *httpx* package from the project.

- `uv add -r requirements.txt`  
  Install dependencies listed in a *requirements.txt* file.  
  > **Note:** Not recommended because *requirements.txt* generally includes all dependencies along with the main packages. It's much cleaner to add only the required packages explicitly. **UV** maintains a separate *uv.lock* file to track package dependencies.

- `uv add --dev pytest, jupyter, ipykernel`  
  Install packages for development.

- `uv lock --upgrade`  
  Upgrade all packages in the project to the latest versions.

- `uv lock --upgrade-package <package>`  
  Upgrade a specific package to its latest version.

- `uv lock --upgrade-package <package>==<version>`  
  Upgrade a package to a specific version.

- `uv sync`  
  Synchronize a project with its dependencies, ensuring that a collaborator is using the same environment setup.

### Running Commands

- `uv run`  
  Run commands in the activated environment. **UV** automatically activates the environment when running commands, so no need for `source activate` and `deactivate` anymore.

---

## Important Files

These are the only two files that **UV** needs to re-create the exact same virtual environment every time. All **UV** commands (like `uv run`, `uv add`, `uv remove`, etc.) will make sure the virtual environment is in sync with the *pyproject.toml* file before performing any action.

> **Note:** Make sure you add these files to git.

- **pyproject.toml**  
  Configuration file used by **UV** to manage project dependencies. It lists allowed Python versions, packages, and their allowed versions.
  
- **lockfile**  
  A file that locks the project dependencies to specific versions.

---

## Python Version Management Commands

**UV** can also manage your Python version, not just the packages in your project. If no specific version is pinned, **UV** will use the default Python version installed on your system.

- `uv python find`  
  Finds first Python executable on your system.

- `uv python list`  
  List all available Python versions for your system.

- `uv python pin`  
  Pin a specific Python version for your project. This command will create/update the `.python-version` file.

  > **Note:** The version in the *.python-version* file should be allowed by the `requires-python` field in the *pyproject.toml* file. 
  
  Here's how to think about it: the *pyproject.toml* file specifies all allowed versions of Python, while the *.python-version* file specifies the specific version of Python to use.

- `uv run --python 3.12 pytest`  
  Run any command using a specific version of Python. This is great for scenarios where you want to run/test your code in multiple Python versions. Handy way to by-pass the pinned python version (if any).

---

## UV Tools & Speed

Because of how fast and easy it is to create disposable virtual environments, **UV** enables many new use cases that were previously not possible. 

- `uvx ruff format .`  
  Run one-off CLI tools without installing them. Great for tasks like code checks, formatting, linting, etc.

- `uv run --with vllm serve Qwen/Qwen2.5-0.5B-Instruct`  
  Run Python CLI packages, again, without installing them. Great for launching long-running servers or batch jobs.

**UV** creates the virtual environment, installs the requested tool, runs the commands, and deletes the virtual environment—all behind the scenes. It's so fast that you won't even notice it. Additionally, you can use the `--with` and `--python` arguments together for even more specific virtual environments.

---

## Conclusion

By migrating to **UV**, you can simplify your Python project management, streamline workflows, and reduce the complexity of using multiple tools. TBH, **UV** will make you rethink your previous practices, CI/CD, and other workflows.
