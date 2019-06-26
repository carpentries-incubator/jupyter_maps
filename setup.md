---
title: Setup
---

> ## Note
> You do not need to install Python and Python packages if you are using a Jupyterhub instance for running this lesson.
> However, you do need to download the data (lesson material) used in the episodes.
>
{: .callout}


### Install Python

In this lesson we will be using Python 3 with some of its scientific libraries.
Although one can install a "plain vanilla" Python 3 and all required libraries "by hand",
we recommend installing [Anaconda][workshop-template-python-instructions], a Python distribution
that comes with everything we need for the lesson.

&nbsp; <!-- vertical spacer -->

### Obtain lesson materials

Follow the steps below. We also give corresponding command lines if you wish to run them from the Terminal:

1. Create a folder called `cc-jupyter-maps`.

~~~
mkdir cc-jupyter-maps
~~~
{: .language-python}

2. Change directory to `cc-jupyter-maps` folder.

~~~
cd cc-jupyter-maps
~~~
{: .language-python}

3. Download [data_jupyter_publish_CarpentryConnect2019.zip](https://zenodo.org/record/3255070/files/data_jupyter_publish_CarpentryConnect2019.tar?download=1).

~~~
curl https://zenodo.org/record/3255070/files/data_jupyter_publish_CarpentryConnect2019.tar?download=1 -o data.tar
~~~
{: .language-python}

4. Unzip the files.

~~~
tar xvf data.tar
~~~
{: .language-python}

You should now see one new folder called `data` in your `cc-jupyter-maps` directory on your
Desktop.

&nbsp; <!-- vertical spacer -->

### Create new conda environment and install Additional python packages

Download [environment.yml](https://raw.githubusercontent.com/annefou/jupyter_maps/master/binder/environment.yml) e.g.
right click and save in your Desktop.

Open a Python Terminal and navigate to your Desktop:

~~~
cd ~/Desktop
~~~
{: .language-python}

Then create a new conda environment and install additional packages:

~~~
conda env create -f environment.yml
~~~
{: .language-python}


[zipfile]: https://zenodo.org/record/3255058/files/data_jupyter_publish_CarpentryConnect2019.tar?download=1
[workshop-template-python-instructions]: https://carpentries.github.io/workshop-template/#python

### JupyterLab extension

If you are using JupyterLab then you need to follow this additional step:

~~~
jupyter labextension install @jupyter-widgets/jupyterlab-manager@0.38
jupyter labextension install jupyter-leaflet
~~~
{: .language-bash}

> ## Note
> These two last commands need to be done within the new `jupyter_maps` python Terminal (not in the default environment).
>
{: .callout}

{% include links.md %}
