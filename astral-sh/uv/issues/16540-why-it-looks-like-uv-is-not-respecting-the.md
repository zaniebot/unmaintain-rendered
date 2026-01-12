```yaml
number: 16540
title: Why it looks like uv is not respecting the manylinux config from PYTHONPATH
type: issue
state: open
author: ccoulombe
labels:
  - question
assignees: []
created_at: 2025-10-31T19:00:10Z
updated_at: 2025-11-03T15:56:32Z
url: https://github.com/astral-sh/uv/issues/16540
synced_at: 2026-01-12T16:02:33Z
```

# Why it looks like uv is not respecting the manylinux config from PYTHONPATH

---

_@ccoulombe_

### Question

Hello,

According to https://docs.astral.sh/uv/pip/compatibility/#manylinux_compatible-enforcement we should be able to ignore any manylinux.

I have this configuration:
```
$ cat $PYTHONPATH/_manylinux.py
manylinux1_compatible = False
manylinux2010_compatible = False
manylinux2014_compatible = False

# Ignore any manylinux_${GLIBCMAJOR}_${GLIBCMINOR}_${ARCH}
def manylinux_compatible(*_, **__):  # PEP 600
    return False
```

But `uv` still install manylinux wheels. What am I missing?

### Platform

Linux

### Version

0.9.2

---

_Label `question` added by @ccoulombe on 2025-10-31 19:00_

---

_Comment by @charliermarsh on 2025-11-01 02:35_

I've confirmed that we successfully omit manylinux wheels with `uv venv`, `uv pip install no-manylinux`, then `uv pip install numpy` (for example), so my guess is that the `PYTHONPATH` you're setting isn't being picked up? I believe we run interpreter discovery under `-I` which ignores `PYTHONPATH`.

---

_Comment by @ccoulombe on 2025-11-03 02:20_

It does indeed work if I install `no-manylinux` in the venv.
```
$ uv pip install --no-build numpy==2.3.4
Installed 1 package in 804ms
 - numpy==2.3.3+computecanada
 ~ numpy==2.3.4
```
Note that the above built the sdist even with the `--no-build` argument provided, which was unexpected. But it did not install a manylinux wheel.

Finally, it does not work with the one from PYTHONPATH 🤔 Here's a diff of the file
```
$ diff $VIRTUAL_ENV/lib/python3.11/site-packages/_manylinux.py $PYTHONPATH/_manylinux.py
5c5
< 
---
> # Ignore any manylinux_${GLIBCMAJOR}_${GLIBCMINOR}_${ARCH}
```
They are essentially the same. And using our `_manylinux.py` file from the `$PYTHONPATH` results in uv installing manylinux wheels.

Any idea of what is going on here? How could `uv` consider the python from the `$PATH` aka the one currently available from the module load.
```
module load python/3.13
uv venv --clear  TESTY
source TESTY/bin/activate
```

Thanks



---

_Comment by @charliermarsh on 2025-11-03 02:27_

Yeah, as I mentioned above, we query the interpreter with `-I` which ignores `PYTHONPATH`. So we don't respect `_manylinux` if it's on `PYTHONPATH` like that.

---

_Comment by @ccoulombe on 2025-11-03 02:55_

Ok, how can we then exclude manylinux wheels? Thanks for the help!!

Unfortunately, we cannot ask every system users to install the `no-manylinux` package :(. 
If only we could minimally seed it into the virtual env. ?

---

_Comment by @charliermarsh on 2025-11-03 02:59_

(Out of interest, can I ask why you want to disable manylinux wheels? It just doesn't come up very often so it's helpful for me to understand.)

---

_Comment by @charliermarsh on 2025-11-03 03:00_

In theory we could add an environment variable for it (like `UV_NO_MANYLINUX`). Would that help?

---

_Comment by @ccoulombe on 2025-11-03 03:07_

An env. variable would greatly help here, or if it could be configured in the config.toml as workaround.

On the HPC systems, we prefer that users use and install our pre-compiled wheels from a specific index (wheelhouse). Those wheels are pre-compiled and optimized for the architectures we have (cpu, gpu). Hence it is faster for users to install them in their compute jobs.
Moreover, those wheels can be built with mpi, openmp or other optimizations,  libraries, features which in many cases the PyPI ones are not.
Lastly, manylinux wheels often do not work on our systems because they can't find the SOs since they are not installed where they are expected (ie `/usr`) but under some lmod modules. 

---

_Assigned to @charliermarsh by @charliermarsh on 2025-11-03 03:14_

---

_Comment by @mboisson on 2025-11-03 13:24_

> (Out of interest, can I ask why you want to disable manylinux wheels? It just doesn't come up very often so it's helpful for me to understand.)

Hi @charliermarsh 
Let me provide some context. We are system administrators for large high performance computing computers across Canada. What this means is that we run large networks of computers, with nearly 20 000 users, all with various needs. It also means that we support a variety of exotic hardware that you would never find on a workstation, such as high speed network, high speed parallel filesystems, and a variety of GPUs. It also means that our software environment is completely non-standard, we need to support thousands of different scientific software packages at the same time, many of which have conflicts. In order to do so, the system libraries are in non-standard locations (and we often have multiple versions of them), and we use what's called environment modules (https://lmod.readthedocs.io/en/latest/) which leverage the users' session' environment variables to make various software packages visible or not, depending on each user's needs. 

The impact of all of this is that wheels that depend on compiled code and which are provided on public indexes will very often have issues, and *we* need to control/recompile them to provide them to our users. If we don't, the result is that users end up installing packages that will not work optimally, or even that will not work at all. So *we* build things like `mpi4py`, using our underlying MPI implementation, *we* build things like `pyarrow`, to leverage arrow compiled and optimized for our hardware, *we* build things like `h5py`, using the underlying compiled `hdf5` libraries, which are built to support MPI-IO, *we* build things like `pytorch` or `tensorflow` to support our CPU and GPU architectures. 

In short, *we* need to control how `pip` (or `uv pip`) behaves for our users, and we use the environment variables to do so. Excluding manylinux wheels (https://github.com/ComputeCanada/software-stack-custom/blob/main/python/site-packages/_manylinux.py) is one mechanism, which we inject into `pip`'s behavior using `PYTHONPATH`, but we also hook into `python` and `pip`'s system directories, using this piece of code: https://github.com/ComputeCanada/easybuild-computecanada-config/blob/main/python/site-packages/sitecustomize.py 
which will pick up environment variables that come from modules in order to find things like `pyarrow` or `mpi4py` or `h5py`, which is the reason for issue #16541.

In short, we don't need `UV_NO_MANYLINUX`, we need `uv` to *not* ignore the environment, to honor `PYTHONPATH` and the site directories that are set by our `sitecustomize.py`. I believe that having a setting to *disable* "querying the interpreter with `-I` to ignore `PYTHONPATH`" may fix most of the issues we have with `uv`. 

There is no issue with using `uv` on a personal computer, but on a high performance computing cluster, system administrators simply need to be able to control its behaviour, otherwise users run into issues and open tickets. 

It also mean, by the way, that we do *not* want `uv python list` to list any python other than the one that is made available by the python module that the user loaded, i.e. if they load `python/3.13`, it should only see that one, not the underlying system python. It also means that having `uv` installing a different version of python than the ones we support is a big no no. 

P.S.: The story that I am telling here is true for supercomputers in Canada, but it is the same on most supercomputers around the world. Different sites may adopt different strategies, but it all boils down to the site administrators need to be able to hook into package managers' and control their behaviour to work with their site. 

---

_Comment by @konstin on 2025-11-03 15:45_

uv checks introspects each Python interpreter once, and then caches the result, to avoid the overhead of running a Python script for each `uv run`. We currently mostly ignore `PYTHONPATH` (through `sys.path`) for the technical complexity where `sys.path` changes e.g. when installing editable packages (https://github.com/astral-sh/uv/pull/11670).

With your description, you can try using `uv.toml` to configure uv to only use system interpreter, not managed interpreter, and using your custom index instead of PyPI.

Do you still have the specific manylinux problem with if you add the `_manylinux` module to the standard library direction of your custom Python interpreter?

---

_Comment by @mboisson on 2025-11-03 15:48_

> uv checks introspects each Python interpreter once, and then caches the result, to avoid the overhead of running a Python script for each `uv run`. We currently mostly ignore `PYTHONPATH` (through `sys.path`) for the technical complexity where `sys.path` changes e.g. when installing editable packages ([#11670](https://github.com/astral-sh/uv/pull/11670)).
> 
> With your description, you can try using `uv.toml` to configure uv to only use system interpreter, not managed interpreter, and using your custom index instead of PyPI.

We do not want to use our custom index *instead* of PyPI, we want to us it *in preference* to PyPI. And not only custom indexes, existing python packages which are present through the environment (through `PYTHONPATH` and the `sys.path`). How do we configure `uv.toml` to get packages from the environment ? 

> Do you still have the specific manylinux problem with if you add the `_manylinux` module to the standard library direction of your custom Python interpreter?

Probably not, but that would be a change to installed packages, which we don't do lightly. Environment variables is the best way to to deal with this. 

That would also not fix the issue that `uv pip` does not see other python packages provided via the environment (which is the bigger problem), i.e. https://github.com/astral-sh/uv/issues/16541. Both issues stem from the same choice of ignoring `PYTHONPATH` and `sys.path`. 

We understand the default behaviour of wanting to be isolated, but this is not something compatible with using `uv` on supercomputers, so we need a way to tell `uv` to consider the environment. 


---
