```yaml
number: 10729
title: jupyterlab-plotly extension not installed when running jupyter from a project with plotly installed
type: issue
state: open
author: MikaelUmaN
labels:
  - question
assignees: []
created_at: 2025-01-18T09:46:25Z
updated_at: 2025-01-23T11:48:50Z
url: https://github.com/astral-sh/uv/issues/10729
synced_at: 2026-01-12T16:00:20Z
```

# jupyterlab-plotly extension not installed when running jupyter from a project with plotly installed

---

_@MikaelUmaN_

I followed this: https://docs.astral.sh/uv/guides/integration/jupyter/

Executing from a project with pyproject.toml as:

```
[project]
name = "app"
version = "0.1.0"
description = "datascience"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "bokeh>=3.6.2",
    "dask-cloudprovider>=2024.9.1",
    "dask-kubernetes>=2025.1.0",
    "dask[dataframe]>=2025.1.0",
    "distributed>=2025.1.0",
    "graphviz>=0.20.3",
    "h5py>=3.12.1",
    "ipywidgets>=8.1.5",
    "numpy>=2.2.1",
    "pandas>=2.2.3",
    "plotly>=5.24.1",
    "pyarrow>=19.0.0",
    "s3fs>=2024.12.0",
    "scikit-learn>=1.6.1",
    "seaborn>=0.13.2",
    "tables>=3.10.2",
    "torch>=2.5.1",
]
[tool.uv.sources]
torch = [
    { index = "pytorch-cpu" },
]
torchvision = [
    { index = "pytorch-cpu" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[dependency-groups]
dev = [
    "ipykernel>=6.29.5",
]
```

This is from a docker image (src: https://github.com/MikaelUmaN/public-images/blob/main/datascience.docker)

I tried running

```uv run --with jupyter  jupyter lab```

Which does work. But, the jupyterlab-plotly extension is then not installed and missing. Subsequently any plotly plots cannot be shown. Plotly is installed correctly, however:

![Image](https://github.com/user-attachments/assets/97f3a82f-ebc6-427c-aedc-7872252d1f4d)

In order to get the plotly extension installed I had to do:

```uv run --with jupyter --with plotly jupyter lab```

which installs the extension: 

![Image](https://github.com/user-attachments/assets/e419978b-ffbe-4600-b21b-061cd656bef6)

But according to the docs, I find this unintuitive since I've already installed plotly (see the docker file); as plotly states that installing plotly does install the extension as well.

1. Am I correct in running ```uv run --with jupyter  jupyter lab``` even though I have also installed jupyter as a tool?
2. Is it by design that we would need to reinstall other extensions (like the plotly jupyterlab extension) when starting jupyterlab - even though we are running inside a project with it's venv?


---

_Renamed from "Plotly-extension not installed when running jupyter" to "jupyterlab-plotly extension not installed when running jupyter from a project with plotly installed" by @MikaelUmaN on 2025-01-18 09:49_

---

_Comment by @charliermarsh on 2025-01-18 15:36_

It seems like you _do_ need to install `plotly` (or related extensions) when running Jupyter as a tool or via `--with` (even if `plotly` is already installed in the underlying environment), yes. It must have to do with how Jupyter detects and leverages the extensions. My guess is the extensions run in the host rather than the kernel? So while `uvx jupyter` can be used to run Jupyter in one environment and use a notebook tied to another environment, I guess it can't access extensions that would only be available in that other environment.

---

_Label `question` added by @charliermarsh on 2025-01-18 15:36_

---

_Comment by @AKuederle on 2025-01-23 11:45_

Jupyter Extension often have two components. They have a part that runs on the host and a part that runs in your kernel. These extensions need to be installed on both sides and often with the exact same version.

This is not a new problem caused by `uv`. This problem always happens when you use the same global jupyter installation for multiple kernels (which makes a lot of sense, as you might not want to have a full jupyter lab installation in every single project).

Below is a copy and paste from some internal documentation that I wrote to help people setup their python envs/switch to uv. I hope that helps!

## Jupyter Lab

One of the tools that I like to have globally installed is Jupyter Lab.
As it is quite a complex tool with many dependencies, I don't want to install it in every project.

### Installation

This installs jupyterlab and the `ipympl` extension globally on your system.
See more information about `ipympl` below.

```
uv tool install --with ipympl --with jupyterlab jupyter-core
```

In case you need to install more python based plugins (e.g. `jupytext`, `jupyter-bokeh`) add additional `--with` flags.

```
uv tool install --with ipympl --with jupytext --with jupyter-bokeh --with jupyterlab jupyter-core
```

For regular nodejs based plugins, you can use the `jupyter labextension install` command as usual.

To update jupyter and all injected dependencies, you can use the `upgrade` command:

```
uv tool upgrade jupyter-core
```

You can now start Jupyterlab from anywhere on your system using the following command:

```
jupyter lab
```

Note, that `jupyter-lab` (with dash instead of space) will not work with this setup!

### Registering local kernels

Now that you have a global juptyerlab installation, you will need to register the venvs of your local projects as seperate kernels.
For this, add `ipykernel` to your projects dev-dependencies and run the following command:

```
uv add ipykernel --group dev
uv run python -m ipykernel install --user --name <name-of-your-project>
```

In Jupyterlab, you can now select the kernel you just registered.

### The ipympl dilemma

The `ipympl` extension is a great tool to use matplotlib in Jupyterlab.
It allows you to zoom and pan in the plots created in your notebook (https://github.com/matplotlib/ipympl).

To make this possible, the extension has two components:
A Jupyterlab extension (running in the jupyterlab GUI) and a python package (running in the kernel).
Because in our setup, Jupyterlab is running in a different Python environment than our kernels.
In result, the `ipympl` extension is not available in our kernels, and we need to add it to the dev-dependencies of our projects.

In this step we need to be extremely careful to pick the exact same version of the `ipympl` extension for Jupyterlab and for our kernels.
In case you run into problems (i.e. the plots are not showing), you should first figure our which version of `ipympl` is installed in your Jupyterlab environment and which is installed in your kernel environment.
Then you should make sure that both versions are the same.

For this, start the default kernel of your jupyterlab installation (usually called `Python 3 (ipykernel)`) and then run `import ipympl; print(ipympl.__version__)` in a cell.
Then repeat the same in the kernel of your project.

If the versions are different, we usually recommend updating the `ipympl` version in both environments to the latest version.
For your global installation run:
    
```
uv tool upgrade jupyter-core
```

and then in your local environment run:

```
uv add ipympl@latest --group dev
```

Then restart jupyterlab and your kernel and check if the versions are now the same and if the plots are showing.
If not, you might run into a different issue.
Check the browsers javascript console for errors and head over to the [ipympl github page](https://github.com/matplotlib/ipympl) for more information.

#### Maintaining multiple versions of ipympl

In case you can not easily update the version of ipympl in your project (e.g. because of reproducibility reasons), we need a way to temporarlily switch the version of ipympl in our global installation.
Luckily, we can do this with a one time execution of jupyter lab with `uv tool run`:

```
uvx --from jupyterlab --with "ipympl==0.7.0" jupyter-lab
```

Note, that this will not install the new version of ipympl in your global installation or modify your global installation in any way.
This means, if you want other extensions to be available, you need to list them explicitly in the command.

You can use this technique to also create a custom jupyterlab execution command for every project, if it requires specific extensions or versions of extensions.

---
