---
layout: post
title: Setting up an Anaconda environment
tags: [Introduction, Anaconda, NumPy, Scikit-learn, Python]
---
In this post I will show you how to install & set-up Anaconda and use it in a Python IDE. I am running on a Mac OS X so commands and details may vary depending on your OS. 

# What is Anaconda?
Anaconda is a free and open-source distribution of python. It comes already with a lot of useful packages for data science installed. Furthermore, it lets you install additional packages easily with the Conda package manager. 
Here is an overview of what comes included in the distribution. 

![Anaconda Distribution]({{ site.baseurl }}/assets/img/Anaconda-Distribution-Diagram.png)

For further detail check out the [Anaconda website](https://www.anaconda.com/).

# How to install Anaconda?
Go to the [download section](https://www.anaconda.com/download) and choose the right installer for your OS. Follow through with the normal installation steps. Once you are done with that, you can check whether the installation was succesful by typing the following into your console:
```python
conda -V
> conda 4.4.10
```
If everything was succesful you should see the version number.

Another cool thing that comes included in the distribution is the Anaconda Navigator. By clicking on *Environment* you get a list of installed packages. 

![Anaconda Navigator]({{ site.baseurl }}/assets/img/Anaconda-Navigator.png)

Some of the most frequently used packages for data scientists are **NumPy, Pandas, Matplotlib and scikit-learn**. These packages are now presented a little bit more in detail. However, since this post is mainly concerned with setting up an Anaconda environment, I won't go much into detail. 

# Included packages
## NumPy
NumPy is a package that helps you n-dimensional data structures like arrays and offers some useful built-in functions for these.
## Pandas
Pandas is a library that offers easy to use data structures and data analysis and visualization tools. 
## Matplot lib
Matplotlib is Python 2D plotting library which produces high-quality figures and grpahs. 
## scikit-learn
Scikit-learn offers simple and efficient tools for data mining and machine learning 

