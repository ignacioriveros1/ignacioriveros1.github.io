---
title:  "Using environments for data projects: A guide for Begginers"
layout: post
date:   "2021-03-08"
categories: ["Coding"]
---

## Introduction
Put here the content

## Table of Contents
1. [Creating an environments using `venv`.](#creating-an-environments-using-venv)
2. [Creating an environments using `conda`.](#creating-an-environments-using-conda)
3. [My personal workflow](#my-personal-workflow)
4. [Final Remarks](#final-remarks)
5. [References](#references)

## Creating and managing environments using `venv`.
<!-- Assuming that you already have `python` installed in your machine (you can check running `python --version`) -->
{% highlight bash %}
# 1. Update pip package manager:
#  Mac/Linux: 
$ (base) python -m pip install --user --upgrade pip
# Windows: 
$ (base) python -m pip install --upgrade pip

# 2. Install virtualenv.
# Mac/Linux: 
$ (base) python -m pip install --user virtualenv
# Windows: 
$ (base) python -m pip install --user virtualenv

# 3. Using your terminal go to the folder of the project where you are working:
$ (base) cd path/to/your/cool/project

# 4. Now, you can create a virtual environment using 
$ (base) python -m venv your-env-name
  
# 5. Activate the environment:
# Mac/Linux:
$ (base) source your-env-name/bin/activate
# Windows:
$ (base) your-env-name\Scripts\activate.ps1
{% endhighlight %}

Congrats!! You just created an environment. If you are using Anaconda, your terminal probably will look like this:
{% highlight bash %}
$ (base)(your-env-name)
{% endhighlight %}

Now, we can begin installing packages in our virtual environment. For illustration purposes, we will use one of the most critical packages to perform scientific computing: [`NumPy`](https://numpy.org){:target="_blank"}.


{% highlight bash %}
# Check the installed packages
$ (base)(your-env-name) pip freeze 

# Install the latest version of numpy
$ (base)(your-env-name) pip install numpy

# Install a specific version:
$ (base)(your-env-name) pip install numpy==1.17.2

# Install a version greater than or equal to a specific one:
$ (base)(your-env-name) pip install numpy>=1.17.2

# Upgrade a package to a newer version:
$ (base)(your-env-name) pip install --upgrade numpy
{% endhighlight %}

There are many variations and commands to install packages using `pip`. Here is the link to the [documentation](https://packaging.python.org/tutorials/installing-packages/){:target="_blank"}. 

It is common to install multiple packages for a project. In addition to `numpy`, let's imagine you need to work with dataframes ([`pandas`](https://pandas.pydata.org){:target="_blank"}) and graphs ([`NetworkX`](https://networkx.org){:target="_blank"}). You can specify a `requirements.txt` file to manage all the packages that you will require in one place. 

{% highlight bash %}
# Location of this file: path/to/your/cool/project/requirements.txt
networkx>=2.4
pandas>=1.1.0
numpy>=1.17.2
{% endhighlight %}

Install all the packages in `requirements.txt` using the following command:
{% highlight bash %}
$ (base)(your-env-name) pip install -r requirements.txt
{% endhighlight %}


Finally, to deactivate the environment or delete it:
{% highlight bash %}
# Deactivate the environment
$ (base)(your-env-name) deactivate

# Delete the environment
# Mac/Linux:
$ (base) rm -rf your-env-name
# Windows:
$ (base) Remove-Item -LiteralPath "your-env-name" -Force -Recurse

{% endhighlight %}


## Creating an environments using `conda`.
We already have some idea of how to use `venv` together with `pip` to manage environments and packages. An alternative and widely used form to achieve the same is use `conda`. As we discussed earlier, `conda` is both an environment and package management system, so you can create environments and install packages just with `conda`. Depending on your operative system, click [here](https://conda.io/projects/conda/en/latest/user-guide/install/index.html){:target="_blank"} to install `conda`. Once installed, let see how to manage our environment!

{% highlight bash %}
# 1. Check if conda was installed correctly
# This command will show all the installed packages in base ...
$ (base) conda list 

# ... and this will show all the environments
$ (base) conda env list 

# 2. Create a new environment with an specific python version
$ (base) conda create -n your-env-name python=3.8

# You can create the environment with some packages
$ (base) conda create -n your-env-name python=3.8 networkx pandas>=1.1.0

# Activate (deactivate) the environment
$ (base) conda activate your-env-name

$ (my-env) conda deactivate

{% endhighlight %}

By default, all your environments will live in a directory inside your conda directory. For example, in my machine the environment was saved in `/Applications/anaconda3/envs/your-env-name`. This approach is different for the one followed by `venv`, because the latter creates the environment in the same folder of the project. This approach has (as everything in life) pros and cons. 

<!-- TODO: Write cons -->
**Pros**
1. Pro 1
<!-- In my opinion, one of the pros is that you can create "general purpose environments" to use in multiple projects (e.g. an enviroment oriented to machine learning). This environment can be called from any directory in your machine.  -->

**Cons**
1. Cons 1

{% highlight bash %}
# Create and env in an specific directory
$ (base) cd path/to/your/project
$ (base) conda create -p ./your-env-name
# or alternatively
$ (base) conda create -p path/to/your/project/your-env-name

# To activate this environment
$ (base) conda activate -p path/to/your/project/your-env-name

{% endhighlight %}

As a package manager, `conda` installed packages from [Anaconda Repository](https://repo.anaconda.com){:target="_blank"} by default. You can also install packages from third parties repositories ([Conda-Forge](https://conda-forge.org){:target="_blank"} is the most popular one) and also from `pip`. This is how it works!

{% highlight bash %}
# IMPORTANT!
# Remember to activate the environment before installing packages
$ (base) conda activate -n your-env-name   # -f path/to/your/project/your-env-name

# Install a package from the Anaconda repo
$ (your-env-name) conda install numpy

# Install a package from conda forge
$ (your-env-name) conda install -c conda-forge numpy

# ... and add the channel to the configuration
$ (your-env-name) conda config --append channels conda-forge

# you can also define which channel to prioritize
$ (your-env-name)  conda config --set channel_priority strict

# Try to avoid pip if you are using conda!
$ (your-env-name)  pip install numpy

# Install a requirements.txt file
$ (your-env-name) conda install --file requirements.txt

{% endhighlight %}

One thing that I found amazing about managing environments with `conda` is that you can specify every aspect of the configuration in a single `.yml` file. For example, let's assume that you have a `environment.yml` configuration:

```yaml
name: your-env-name
channels:
  - conda-forge
  - defaults
dependencies:
- python=3.7
- pytest
- ipykernel
- ipython
- pip:
    - autopep8==1.5.4
    - dropbox==10.4.1
    - paramiko==2.7.2
    - redis==3.5.3
    - twilio==6.41.0
```

With this file you can create environments using:

{% highlight bash %}
# To create the env
$ (base) conda env create -n your-env-name -f /path/to/environment.yml

# To update the env
$ (base) conda env update -n conda-env -f /path/to/environment.yml

{% endhighlight %}

Also, if you want to save the specification of an environment in a `.yml` you can do it!
{% highlight bash %}

$ (base) conda activate your-env-name
$ (your-env-name) conda env export > your-env-name.yml

{% endhighlight %}

For more specific details, [read the docs](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html){:target="_blank"}!

## My personal workflow.
There are several packages that I always want to have in any environment I use. Here is the specification :)!

```yaml
name: null
channels:
  - conda-forge
  - defaults
dependencies:
- python>=3.7 #  Specify your desire version
- ipython # To develop and run my code
- pytest # To perform testing to the code 
- autopep8 # Code formatter for the PEP8 guidelines
- mypy # Static typing
- flake8 # For linting (spell and grammar checker of python code)
prefix: path/to/env/conda-env
```

## Final Remarks



## References
* [The Definitive Guide to Conda Environments](https://towardsdatascience.com/a-guide-to-conda-environments-bc6180fc533){:target="_blank"}
* [Python Virtual Environments](https://uoa-eresearch.github.io/eresearch-cookbook/recipe/2014/11/26/python-virtual-env/){:target="_blank"}
* [Data Science Best Practices: Python Environments](https://towardsdatascience.com/data-science-best-practices-python-environments-354b0dacd43a){:target="_blank"}
* [GCPy: Python-powered GEOS-Chem analysis/visualization](http://danielrothenberg.com/gcpy/getting_started.html){:target="_blank"}
* [Python Packaging User Guide: Tutorials, Installing Packages](https://packaging.python.org/tutorials/installing-packages/){:target="_blank"}
* [Python Environments](https://rabernat.github.io/research_computing_2018/python-environments.html){:target="_blank"}