---
title:  "Using environments for data projects: A guide for Begginers"
layout: post
date:   "2021-03-08"
categories: ["Coding"]
---

## Introduction
Put here the content

## Table of Contents
1. [Background: Environments and Package Managers.](#background:environments-and-package-managers)
2. [Creating an environments using `venv`.](#creating-an-environments-using-venv)
3. [Creating an environments using `conda`.](#creating-an-environments-using-conda)
4. [Final Remarks](#final-remarks)
5. [References](#references)

## Background: Environments and Package Managers.
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
## Final Remarks





## References

* [The Definitive Guide to Conda Environments](https://towardsdatascience.com/a-guide-to-conda-environments-bc6180fc533){:target="_blank"}
* [Python Virtual Environments](https://uoa-eresearch.github.io/eresearch-cookbook/recipe/2014/11/26/python-virtual-env/){:target="_blank"}
* [Data Science Best Practices: Python Environments](https://towardsdatascience.com/data-science-best-practices-python-environments-354b0dacd43a){:target="_blank"}
<!-- http://danielrothenberg.com/gcpy/getting_started.html -->
<!-- https://packaging.python.org/tutorials/installing-packages/ -->