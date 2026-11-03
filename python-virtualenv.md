# Python Virtual Environment

## 1. What is a virtual environment?

A virtual environment is a separate Python environment created for a project. It keeps the project's installed packages separate from other projects and the system Python installation.

For example, one project may need an older version of a package, while another project needs the latest version. Virtual environments help us manage both projects without package conflicts.

## 2. Why do we use it?

- **Avoid package conflicts:** Each project can use its own package versions.
- **Keep projects separate:** Packages installed for one project do not affect another.
- **Protect system Python:** Project packages stay away from the system installation.
- **Make projects easier to share:** We can record dependencies in a `requirements.txt` file.

## 3. Create a virtual environment

Open the terminal inside your project folder and run:

```bash
python -m venv .venv
```

- `python` selects the Python interpreter.
- `-m venv` runs Python's built-in virtual environment module.
- `.venv` is the folder where the environment is created.

You can use another folder name, but `.venv` is a common choice.

## 4. Activate the environment

Choose the command for your operating system.

**Linux / macOS**

```bash
source .venv/bin/activate
```

**Windows Command Prompt**

```bat
.venv\Scripts\activate.bat
```

**Windows PowerShell**

```powershell
.venv\Scripts\Activate.ps1
```

After activation, the environment name usually appears at the beginning of the terminal prompt.

## 5. Work inside the environment

Check the Python version:

```bash
python --version
```

Install a package:

```bash
python -m pip install pandas
```

Check installed packages:

```bash
python -m pip list
```

Packages installed now belong to the active environment.

## 6. Deactivate the environment

When you finish working, run:

```bash
deactivate
```

This leaves the virtual environment. It does not delete it.

## 7. Remove a virtual environment

First deactivate it. Then delete the `.venv` folder from your project.

You can create it again whenever you need it.

## 8. Useful project practices

- Create one virtual environment for each Python project.
- Add `.venv/` to your `.gitignore` file.
- Do not upload the virtual environment folder to GitHub.
- Keep a `requirements.txt` file to record the packages used by the project.
- Activate the environment before installing or running project packages.

## Quick revision

| Task | Command |
|---|---|
| Create | `python -m venv .venv` |
| Activate on Linux / macOS | `source .venv/bin/activate` |
| Activate on Windows CMD | `.venv\Scripts\activate.bat` |
| Deactivate | `deactivate` |
| Check Python | `python --version` |

## Remember

A virtual environment does not install a separate operating system or a completely different Python language. It provides an isolated place to manage Python packages for a project.
