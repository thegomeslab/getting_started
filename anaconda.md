# Setting Up a New Anaconda Python Environment

These instructions explain how to create and use a new Python environment with Anaconda/conda. They are written for students who are just getting started with terminal-based workflows.

A **conda environment** is an isolated Python workspace with its own Python version and packages. This is useful because different projects often need different package versions, and environments keep those projects from interfering with each other.

Useful references:

- Conda environment documentation: <https://docs.conda.io/projects/conda/en/stable/user-guide/tasks/manage-environments.html>
- Anaconda installation documentation: <https://www.anaconda.com/docs/getting-started/anaconda/install/overview>
- Miniconda installation documentation: <https://docs.conda.io/projects/conda/en/stable/user-guide/install/index.html>

---

## 1. What is Anaconda? What is conda?

**Anaconda** is a Python distribution commonly used for scientific computing. It includes Python, the `conda` package manager, and many tools useful for data science and computational work.

**conda** is the command-line program that creates environments and installs packages.

You will usually interact with Anaconda by typing commands into a terminal.

Common commands look like this:

```bash
conda create -n myenv python=3.11
conda activate myenv
conda install numpy scipy matplotlib
```

In these notes, commands are shown in code blocks. Type the command exactly as shown, then press **Enter**.

---

## 2. Open a terminal

### On macOS or Linux

Open the **Terminal** application.

### On Windows

Use one of the following:

```text
Anaconda Prompt
```

or

```text
PowerShell
```

If you are new to Anaconda on Windows, the **Anaconda Prompt** is often easiest.

---

## 3. Check whether conda is already installed

In your terminal, type:

```bash
conda --version
```

You should see something like:

```text
conda 25.3.1
```

The exact version number may be different.

You can also check:

```bash
conda info
```

This prints information about your conda installation, including where conda is installed and which environment is currently active.

If the command works, you already have conda installed.

If you see an error like:

```text
conda: command not found
```

then conda is not available in your current terminal session. Either Anaconda/Miniconda is not installed, or your terminal has not been initialized correctly.

---

## 4. Installing Anaconda or Miniconda

There are two common choices.

### Option A: Anaconda Distribution

Anaconda Distribution includes conda, Python, and many scientific packages. This is a large installation but convenient for beginners.

Official installation instructions are available here:

<https://www.anaconda.com/docs/getting-started/anaconda/install/overview>

### Option B: Miniconda

Miniconda is a smaller installation that includes only conda, Python, and a minimal set of packages. You install additional packages as needed.

Official installation instructions are available here:

<https://docs.conda.io/projects/conda/en/stable/user-guide/install/index.html>

For many research computing workflows, **Miniconda is often preferred** because it is smaller and keeps your setup cleaner. For a beginner on a personal computer, either Anaconda or Miniconda is fine.

After installation, close and reopen your terminal, then run:

```bash
conda --version
```

If that works, continue.

---

## 5. Initialize conda for your shell, if needed

Sometimes `conda` is installed but `conda activate` does not work. If you see an error when trying to activate an environment, run:

```bash
conda init
```

Then close and reopen your terminal.

On macOS/Linux, you may need to specify your shell:

```bash
conda init bash
```

or, for newer macOS systems that use `zsh`:

```bash
conda init zsh
```

The `conda init` command modifies your shell startup files so that conda activation works automatically in future terminal sessions.

---

## 6. Do not work in the base environment

When conda first starts, you may see something like this at the beginning of your terminal prompt:

```text
(base)
```

For example:

```text
(base) student@computer:~$
```

The `base` environment is conda's default environment. It is best not to install research-project packages directly into `base`.

Instead, create a separate environment for each project.

Good practice:

```text
base                 Leave mostly untouched
myproject            Environment for one research project
class-env            Environment for a class
ml-env               Environment for machine learning work
```

---

## 7. Create a new environment

The general command is:

```bash
conda create -n ENVIRONMENT_NAME python=PYTHON_VERSION
```

For example, to create an environment named `research` with Python 3.11:

```bash
conda create -n research python=3.11
```

Conda will show you a list of packages it plans to install and ask:

```text
Proceed ([y]/n)?
```

Type:

```text
y
```

and press **Enter**.

---

## 8. Activate the environment

After creating the environment, activate it:

```bash
conda activate research
```

Your terminal prompt should change. For example:

```text
(research) student@computer:~$
```

The environment name in parentheses tells you which environment is currently active.

This is important: packages will be installed into the active environment.

---

## 9. Confirm which Python you are using

With your environment activated, run:

```bash
python --version
```

Example output:

```text
Python 3.11.9
```

You can also check where Python is located.

### macOS/Linux

```bash
which python
```

Example:

```text
/Users/student/miniconda3/envs/research/bin/python
```

### Windows

```bash
where python
```

Example:

```text
C:\Users\student\miniconda3\envs\research\python.exe
```

The path should include the environment name, such as `research`.

---

## 10. Install packages with conda

To install packages, first make sure your environment is active:

```bash
conda activate research
```

Then install packages:

```bash
conda install numpy scipy matplotlib pandas
```

Conda will solve the environment, show the packages to be installed, and ask for confirmation. Type `y` and press **Enter**.

The general form is:

```bash
conda install package_name
```

or for multiple packages:

```bash
conda install package1 package2 package3
```

---

## 11. Example: create a useful scientific Python environment

Here is a reasonable starter environment for scientific computing:

```bash
conda create -n research python=3.11
conda activate research
conda install numpy scipy matplotlib pandas jupyter ipython
```

This installs:

```text
numpy       numerical arrays
scipy       scientific computing tools
matplotlib  plotting
pandas      tables and data analysis
jupyter     notebooks
ipython     improved interactive Python shell
```

You can test the installation by running:

```bash
python
```

Then type:

```python
import numpy as np
import scipy
import matplotlib
import pandas as pd

print("Everything imported successfully!")
```

Exit Python with:

```python
exit()
```

or press:

```text
Ctrl-D
```

on macOS/Linux.

On Windows, you can usually use:

```text
Ctrl-Z
```

then press **Enter**.

---

## 12. Install packages with pip when needed

Some Python packages are not available through conda or may need to be installed with `pip`.

First activate the environment:

```bash
conda activate research
```

Then use:

```bash
python -m pip install package_name
```

For example:

```bash
python -m pip install ase
```

Using `python -m pip` is safer than just typing `pip` because it ensures pip belongs to the currently active Python environment.

Good practice:

```bash
conda activate research
python -m pip install some_package
```

Avoid installing packages with pip into the `base` environment.

---

## 13. Check which packages are installed

To list packages in the active environment:

```bash
conda list
```

This prints all packages installed in the current environment.

To check whether a specific package is installed, you can use:

```bash
conda list numpy
```

or:

```bash
python -c "import numpy; print(numpy.__version__)"
```

---

## 14. Start Jupyter Notebook

If you installed Jupyter:

```bash
conda activate research
jupyter notebook
```

This should open a browser window.

To stop Jupyter, return to the terminal and press:

```text
Ctrl-C
```

You may need to press it twice or confirm with `y`.

---

## 15. Start JupyterLab

If JupyterLab is installed:

```bash
conda activate research
jupyter lab
```

If it is not installed, install it:

```bash
conda install jupyterlab
```

Then run:

```bash
jupyter lab
```

---

## 16. Deactivate the environment

When you are done working, deactivate the environment:

```bash
conda deactivate
```

Your prompt should return to something like:

```text
(base)
```

or the environment name may disappear entirely.

---

## 17. See all of your conda environments

To list all environments:

```bash
conda env list
```

or:

```bash
conda info --envs
```

You will see output like:

```text
# conda environments:
#
base                  *  /Users/student/miniconda3
research                 /Users/student/miniconda3/envs/research
```

The `*` marks the currently active environment.

---

## 18. Remove an environment

If you make a mistake or no longer need an environment, remove it with:

```bash
conda remove -n research --all
```

Conda will ask for confirmation.

Be careful: this deletes the environment and the packages installed in it. It does **not** delete your project files unless you stored them inside the environment directory, which you usually should not do.

---

## 19. Update packages

To update a single package:

```bash
conda update numpy
```

To update conda itself:

```bash
conda update conda
```

In research projects, avoid updating packages casually in the middle of a project unless you have a reason. Updating packages can sometimes change results or break compatibility.

---

## 20. Save your environment to a file

It is good practice to save your environment so someone else can recreate it later.

Activate the environment:

```bash
conda activate research
```

Then export it:

```bash
conda env export > environment.yml
```

This creates a file named:

```text
environment.yml
```

You can share this file with another person or include it in a GitHub repository.

---

## 21. Recreate an environment from `environment.yml`

If someone gives you an `environment.yml` file, go to the folder containing that file and run:

```bash
conda env create -f environment.yml
```

Then activate the environment:

```bash
conda activate ENVIRONMENT_NAME
```

The environment name is usually listed near the top of the `environment.yml` file:

```yaml
name: research
```

---

## 22. A simple daily workflow

Every time you start working on the project:

```bash
cd path/to/your/project
conda activate research
python script.py
```

For example:

```bash
cd ~/projects/my-research-project
conda activate research
python analyze_data.py
```

When finished:

```bash
conda deactivate
```

---

## 23. Recommended project folder structure

A simple project might look like this:

```text
my-research-project/
├── README.md
├── environment.yml
├── data/
├── scripts/
│   └── analyze_data.py
├── notebooks/
│   └── exploration.ipynb
└── results/
```

The conda environment itself should **not** be stored inside your project folder. Let conda manage environments in its normal environment directory.

---

## 24. Common beginner mistakes

### Mistake 1: Installing into the wrong environment

Before installing packages, always check your prompt:

```text
(research)
```

or run:

```bash
conda info --envs
```

Make sure the correct environment has the `*`.

### Mistake 2: Forgetting to activate the environment

If Python cannot find a package that you installed, you may not have activated the environment:

```bash
conda activate research
```

### Mistake 3: Using `pip` from the wrong Python

Prefer:

```bash
python -m pip install package_name
```

instead of:

```bash
pip install package_name
```

### Mistake 4: Working in `base`

Avoid:

```bash
conda install lots_of_packages
```

while in `base`.

Instead:

```bash
conda create -n project-name python=3.11
conda activate project-name
conda install lots_of_packages
```

### Mistake 5: Creating too many confusing environments

Use meaningful names:

```text
good:  catalysis-ml
good:  psc-analysis
good:  chem-env
bad:   test
bad:   new
bad:   env2
```

---

## 25. Troubleshooting

### Problem: `conda: command not found`

Try closing and reopening the terminal.

If that does not work, conda may not be installed or may not be initialized. Try:

```bash
conda init
```

If even `conda init` is not found, reinstall Anaconda or Miniconda, or ask for help.

### Problem: `conda activate` does not work

Run:

```bash
conda init
```

Then close and reopen the terminal.

On macOS/Linux, you may need:

```bash
conda init bash
```

or:

```bash
conda init zsh
```

### Problem: package import fails

For example:

```text
ModuleNotFoundError: No module named 'numpy'
```

First check that the environment is active:

```bash
conda activate research
```

Then install the missing package:

```bash
conda install numpy
```

or, if needed:

```bash
python -m pip install numpy
```

### Problem: Jupyter cannot see the environment

Activate the environment and install `ipykernel`:

```bash
conda activate research
conda install ipykernel
python -m ipykernel install --user --name research --display-name "Python (research)"
```

Then restart Jupyter. You should be able to choose:

```text
Python (research)
```

as the notebook kernel.

---

## 26. Minimal command summary

Create an environment:

```bash
conda create -n research python=3.11
```

Activate it:

```bash
conda activate research
```

Install packages:

```bash
conda install numpy scipy matplotlib pandas jupyter
```

Check Python:

```bash
python --version
```

Run Python:

```bash
python
```

List installed packages:

```bash
conda list
```

Export the environment:

```bash
conda env export > environment.yml
```

Deactivate:

```bash
conda deactivate
```

Remove the environment:

```bash
conda remove -n research --all
```

---

## 27. Example full setup from scratch

This is a complete example a student can copy and adapt:

```bash
# Create a new environment named research with Python 3.11
conda create -n research python=3.11

# Activate the environment
conda activate research

# Install common scientific Python packages
conda install numpy scipy matplotlib pandas jupyter ipython

# Check that Python is working
python --version

# Test imports
python -c "import numpy, scipy, matplotlib, pandas; print('Environment is ready!')"

# Save the environment to a file
conda env export > environment.yml
```

After that, the normal workflow is:

```bash
conda activate research
python my_script.py
```

When finished:

```bash
conda deactivate
```


