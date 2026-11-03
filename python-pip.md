# Python pip

## 1. What is pip?

`pip` is Python's package installer. It helps us install, update, and remove external Python libraries.

Python already includes a standard library. When we need additional tools such as `pandas`, `requests`, or `matplotlib`, we can install them using pip.

## 2. Check pip

Check whether pip is available:

```bash
python -m pip --version
```

Using `python -m pip` runs pip through the Python interpreter selected by the `python` command. This helps avoid installing packages into the wrong Python environment.

## 3. Install packages

Install a package:

```bash
python -m pip install pandas
```

Install a particular version:

```bash
python -m pip install pandas==2.2.3
```

Install more than one package:

```bash
python -m pip install pandas matplotlib
```

## 4. Update and remove packages

Upgrade a package:

```bash
python -m pip install --upgrade pandas
```

Uninstall a package:

```bash
python -m pip uninstall pandas
```

pip may ask for confirmation before removing a package.

## 5. Check installed packages

List all installed packages:

```bash
python -m pip list
```

Show information about one package:

```bash
python -m pip show pandas
```

This can show the installed version, package location, and dependencies.

## 6. Use requirements.txt

A `requirements.txt` file records the packages needed by a project. It helps another person install the same dependencies.

Save the current environment's installed packages:

```bash
python -m pip freeze > requirements.txt
```

Install packages listed in the file:

```bash
python -m pip install -r requirements.txt
```

Example `requirements.txt`:

```text
pandas==2.2.3
matplotlib==3.9.2
```

The versions above are examples. Use the versions required by your project.

## 7. Upgrade pip

Run:

```bash
python -m pip install --upgrade pip
```

## 8. Common problems

- **Permission denied:** Use a virtual environment instead of installing project packages globally.
- **Package not found:** Check the package name and your internet connection.
- **Wrong Python environment:** Activate the correct virtual environment and use `python -m pip`.
- **Version conflict:** Check the package requirements and versions installed in the environment.

## Quick revision

| Task | Command |
|---|---|
| Check pip | `python -m pip --version` |
| Install | `python -m pip install package_name` |
| Upgrade | `python -m pip install --upgrade package_name` |
| Uninstall | `python -m pip uninstall package_name` |
| List packages | `python -m pip list` |
| Package details | `python -m pip show package_name` |
| Save dependencies | `python -m pip freeze > requirements.txt` |
| Install dependencies | `python -m pip install -r requirements.txt` |

## Remember

Activate your project's virtual environment before installing packages. This keeps dependencies in the correct project and reduces conflicts with other Python installations.
