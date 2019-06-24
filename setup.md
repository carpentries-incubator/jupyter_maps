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

1. Download [data_jupyter_publish_CarpentryConnect2019.zip](https://zenodo.org/record/3255070/files/data_jupyter_publish_CarpentryConnect2019.tar?download=1).
2. Create a folder called `cc-jupyter-maps` on your Desktop.
3. Move downloaded files into this newly created folder.
4. Unzip the files.

You should now see one new folder called `data` in your `cc-jupyter-maps` directory on your
Desktop.

&nbsp; <!-- vertical spacer -->

### Navigate to the `data` folder

If you're using a Unix shell application, such as Terminal app in macOS, Console or Terminal in
Linux, or [Git Bash](https://gitforwindows.org/) on Windows, execute the following command:

~~~
$ cd ~/Desktop/cc-jupyter-maps/data
~~~
{: .source}

On Windows, you can use its native Command Prompt program.  The easiest way to start it up is by
pressing <kbd>Windows Logo Key</kbd>+<kbd>R</kbd>, entering `cmd`, and hitting <kbd>Enter</kbd>. In
the Command Prompt, use the following command to navigate to the `data` folder:
~~~
$ cd /D %userprofile%\Desktop\cc-jupyter-maps\data
~~~
{: .source}

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
