---
title: 'Creating a Virtual Environment in Python'
date: 2024-12-06
permalink: /posts/2024/12/creating-virtual-environment-python/
tags:
  - Python
  - virtual environment
  - venv
  - virtualenv
---

In the world of Python development, managing dependencies effectively is crucial. One of the best ways to do this is by using virtual environments. Virtual environments allow you to create isolated environments for different projects, ensuring that each project has its own set of dependencies, independent of other projects or system-wide packages. This helps avoid conflicts between different versions of libraries and ensures that your projects remain portable and reproducible.

In this article, we will go through the process of creating and using virtual environments in Python, covering both `venv` (the built-in Python module) and `virtualenv` (a popular third-party tool).

**What is a Virtual Environment?**
A virtual environment is a self-contained directory tree that contains the Python installation for a particular version of Python, as well as additional packages. This isolation prevents the installed libraries in one project from affecting other projects or the system-wide Python installation. 

**Benefits of Using Virtual Environments**
- Dependency Management: Each project can have its own dependencies, even if they conflict with dependencies in other projects.
- Clean Development Setup: No more version conflicts or dependency issues across projects.
- Reproducibility: It makes it easier to recreate a project setup from scratch on another system.
- Experimentation: You can safely experiment with different versions of libraries without affecting other projects.

**Creating a Virtual Environment -- Step by Step**

**Step 1: Install Python**

First, make sure Python is installed on your system. You can download Python from the [official website](www.python.org). To check if Python is installed and verify its version, run:

```
python --version
```
or for Python 3:

```python3 --version```

If Python is not installed, you can follow the installation instructions for your operating system from the official Python website.

**<u>A) Virtual Environment Using `venv` (Built-in Python Module)</u>**

**Step 2: Create a Virtual Environment Using venv**

Python 3.3+ includes a built-in module called `venv` that can be used to create a virtual environment. To create virtual environment using venv, you need to navigate to the directory, create the virtual environment and activate it.

- Navigate to Your Project Directory

First, navigate to the folder where you want to store your project, or create a new folder if needed.

```
mkdir my_project
cd my_project
```

- Create the Virtual Environment

Use the following command to create a new virtual environment. You can name the environment whatever you want (let us name it .venv here). I prefer .venv thank .env because I often use .env for environmet variables.

```
python3 -m venv .venv
```
This will create a directory named `.venv` in your project folder. The `.venv` directory contains a copy of the Python interpreter, along with the standard Python libraries. 

- Activate the Virtual Environment

To activate the virtual environment, you need to run the appropriate command based on your operating system.

| OS    | Code to activate virtual environment |
| -------- | ------------------------ |
| Windows  | `.\env\Scripts\activate`    |
| macOS | `source env/bin/activate`     |
| Linux    | `source env/bin/activate`    |

After activating the virtual environment, your terminal prompt should change, indicating that the virtual environment is now active. For example, on Windows, you might see something like:

```
(env) C:\path\to\my_project>
```

**Step 3: Installing Packages in the Virtual Environment**

Once the virtual environment is active, you can install Python packages using pip, and they will only be available within that environment. For example, to install the requests library, you use `pip install requests` and you can now use requests within your project, but it won't interfere with any other projects you might be working on.

**Step 4: Deactivate the Virtual Environment**

When you're done working in your virtual environment, you can deactivate it by running the `deactivate` command as follows:

```
(env) C:\path\to\my_project>deactivate
```
Your terminal prompt will return to its original state, and you will be back to using the global Python environment.

**Step 5: Requirements File (Optional but Recommended)**

To ensure that other developers (or you, on another machine) can easily recreate the virtual environment with the same dependencies, you can create a `requirements.txt` file. This file lists all the packages currently installed in your environment. This ensures reproduceability and portability of your application. You can generate a requirements.txt file automatically using a handy tool called `freeze` as follows:

```
pip freeze > requirements.txt
```
This will save all installed packages and their versions in `requirements.txt`.

To install the same packages later (or on a different machine), use:

```
pip install -r requirements.txt
```

**Step 6: Delete the Virtual Environment (Optional)**

If you no longer need the virtual environment, you can delete the entire `.venv` directory. Simply remove it like any other directory:

| OS    | Code to activate virtual environment |
| -------- | ------------------------ |
| Windows  | `rmdir /S /Q .venv`    |
| macOS | `rm -rf .venv`     |
| Linux    | `rm -rf .venv`    |

This will completely remove the virtual environment and all its contents.

**<u>B) Virtual Environment Using `virtualenv` (Third-Party Tool)</u>**

While `venv` is built into Python 3, you may want to use virtualenv, a popular third-party tool that provides some additional features, like compatibility with Python 2.

**Step 1: Install `virtualenv`**

If you don't already have `virtualenv` installed, you can install it globally using pip:

```
pip install virtualenv
```

**Step 2: Create a Virtual Environment with virtualenv**

To create a new virtual environment, run the following command:

```
virtualenv .venv
```

This will create a new virtual environment in the `.venv` directory.

**Step 3: Activate and Deactivate the Environment**

The activation and deactivation steps are the same as with `venv`. After activation, you can install packages using pip just like before.

**Conclusion**

Creating and managing virtual environments is an essential practice for any Python developer. By using `venv` or `virtualenv`, you ensure that your projects remain isolated and that dependencies don't conflict with each other or with the base system. This isolation simplifies the development process, allows for easier dependency management, and enhances reproducibility, making it easier to share your projects with others.

Remember to use a `requirements.txt` file to document your project’s dependencies and make it easy to recreate the environment on another machine.

If you’re working on multiple projects, adopting the practice of using virtual environments will help you maintain cleaner, more manageable codebases.

Remember to also add .venv into your .gitignore file to exclude the syncronization the virtual environment to your git repository (avoid unnecessary bandwidth usage).

---

Now that you have read the article till the end, did you find this blog post useful? Drop me your comment as a message on [LinkedIn](www.linkedin.com/in/sisayie)