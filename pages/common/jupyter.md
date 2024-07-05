# jupyter

> Web application to create and share documents that contain code, visualizations and notes.
> Primarily used for data analysis, scientific computing and machine learning.
> More information: <https://jupyter.org>.

- Start a Jupyter notebook server in the current directory:

`jupyter notebook`

- Open a specific Jupyter notebook:

`jupyter notebook {{example.ipynb}}`

- Export a specific Jupyter notebook into another format:

`jupyter nbconvert --to {{html|markdown|pdf|script}} {{example.ipynb}}`

- Start a server on a specific port:

`jupyter notebook --port={{port}}`

- List currently running notebook servers:

`jupyter notebook list`

- Stop the currently running server:

`jupyter notebook stop`

- Start JupyterLab, if installed, in the current directory:

`jupyter lab`



# Custom ...........................................................................................
## Installation/Configuration
[The Best Way to Install Jupyter Lab (it's Pipx) | Sam Edwardes](https://samedwardes.com/2022/10/23/best-jupyter-lab-install/#step-2-install-jupyter-lab-with-pipx)

```bash
## Global
pipx install notebook --include-deps

## Charm
poetry add -G jupyter jupyter
poetry add -G jupyter jupyterlab_vim  # looks like plugins are just inatlled with pip

pdm add notebook
pdm add ipywidgets

jupyter labextension list

## Configuration
jupyter notebook --generate-config  # create jupyter_notebook_config.py ~/.jupyter/.
# edit jupyter_notebook_config.py
```

## Operations
```
- magics and bash calls return python variables
%env VAR=XXX  # (except PATH)
?str.replace()  # get help

# return python variables
names = !ls ../images/ml_demonstrations/*.png
names[:5]

from IPython.display import display, Image
for name in names[:5]:
    display(Image('../images/ml_demonstrations/' + name, width=100))
```
## Magics
```
%lsmagic

# quick reference guide
%quickref

%matplotlib inline
%who

# list all environment variables or specific variables
%env SYSTEMROOT

# print the call signature for any object
import urllib
%pdef urllib.urlopen
%pdoc urllib.urlopen

%reset

%%capture output
output.show()
```

## Themes
```bash
jt -T -t grade3
```

## Install Kernel
https://queirozf.com/entries/jupyter-kernels-how-to-add-change-remove
```bash
# add kernel in virtual environment
$ python -m venv projectname
$ source projectname/bin/activate
(venv) $ pip install ipykernel  # Gotcha: must be done manually
(venv) $ ipython kernel install --user --name=projectname

# remove kernel
jupyter kernelspec list
jupyter kernelspec uninstall powerpool
```

## Shortcuts
```
-/M:        split/merge cell
Shift ESC:  blue mode
CMD 1/2:    switch code/markdown
```

