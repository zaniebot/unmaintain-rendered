---
number: 6329
title: "Consider adding documentation on using `uv ...` to use a `.venv` in Jupyter notebooks"
type: issue
state: closed
author: prrao87
labels:
  - documentation
assignees: []
created_at: 2024-08-21T14:16:52Z
updated_at: 2025-02-02T05:48:58Z
url: https://github.com/astral-sh/uv/issues/6329
synced_at: 2026-01-10T01:23:59Z
---

# Consider adding documentation on using `uv ...` to use a `.venv` in Jupyter notebooks

---

_Issue opened by @prrao87 on 2024-08-21 14:16_

Thank you for all the amazing updates!

I've successfully migrated my project workflows over to use `uv python` and `uv run`, and this works great. As a user who (sometimes) has to use Jupyter notebooks, I find it a little hard to understand how I can use uv to manage Python versions and virtual environments that are accessible from within the Jupyter notebook.

I did see that it's possible to use `uv tool` to achieve this in some form, but it's not clear to me how this would work.
https://github.com/astral-sh/uv/issues/4994#issuecomment-2241813473

When I run the command `uv tool run --from jupyter-core --with jupyter jupyter notebook`, indeed it does open a Jupyter notebook, but it's not clear if I can open a new notebook that's able to use the local virtual environment in that directory or the dependencies specified in `uv.lock`.

Could there be a documentation section added that explains how users can access a uv virtual environment and/or `uv.lock` from within a Jupyter notebook?



---

_Comment by @baggiponte on 2024-08-21 14:28_

Uhm, that's a good question. I think it might be a bit tough since exposing a venv to jupyter requires ipykernel in the venv and running a specific ipykernel command [to create the kernel].

But on twitter there was [this reply](https://x.com/strangemonad/status/1826080686203449633) and Charlie saying that "@konstin has wanted to do this forever".

---

_Comment by @prrao87 on 2024-08-21 14:34_

That's exactly the tweet I wanted to see, thanks for pointing it out! I hope this feature is considered, as this is, for me, is the FINAL step in completely leaving pyenv, poetry and pip and forever using uv in all my workflows. I think this is true for > 90% of users, too. 😎

---

_Comment by @bluss on 2024-08-21 14:37_

Mentioning this, because it hits pretty close to the issue..

I've worked on https://bluss.github.io/pyproject-local-kernel/ which basically is that support, using Uv (or Rye, or others) to define dependencies per notebook project. It's made specifically for JupyterLab (works same way in Jupyter Notebook).

By default when you run JupyterLab (or jupyter notebook), it runs a bundled ipykernel which has access to exactly the dependencies installed together with jupyter. You'll need to do something extra to change this.

If you run jupyterlab with pyproject-local-kernel:

`uvx --with pyproject-local-kernel --from jupyterlab jupyter-lab`

Then you have support for using uv projects to define notebook dependencies.

This is something that works for me/my team, we it's easy to check out notebooks from git and have separate directories (projects) with separate dependencies.
There are other ways to do it:

- Install a kernelspec per virtualenv (or other python env, conda, etc)
- VS Code, already natively supports uv's `.venv`.
- **Jupyterlab Desktop** has a virtualenv selector built-in (but note that its selector is for Jupyter + the notebook deps together).


(1) ~~uvx has the same bug/problem with jupyterlab that was already gracefully fixed for `uv run`, which is that uvx dies on Ctrl-C and leaves jupyterlab running in the background.~~ (now, of course fixed :sunglasses: )

---

_Renamed from "Consider adding documentation on using `uv tool ...` for Jupyter notebook users" to "Consider adding documentation on using `uv ...` to use a `.venv` in Jupyter notebooks" by @prrao87 on 2024-08-21 14:43_

---

_Comment by @prrao87 on 2024-08-21 14:53_

Thanks @bluss for the ideas. Just in case anybody else stumbles across this thread, I did the following and it worked for me using Jupyter Lab:

```bash
# Start project which generates .venv
uv init
# Add a sample library
uv add requests
# Add ipykernel to work with notebooks
uv add ipykernel
# Install kernelspec for the current .venv
uv run ipython kernel install --user --name=uv_test
# Run Jupyter Lab and open a new notebook using the newly created kernel `uv_test`
uvx --from jupyter-core --with jupyter jupyter lab
```
Then, I test that my `requests` module import works in the notebook, and it does:
```python
import requests
```

That's a lot of steps to keep in mind, so I hope that a) this is documented somewhere, as this isn't intuitive for a lot of people, and b) the idea from the tweet linked above is implemented and that uv handles a lot of this burden for the user.
All in all, so happy we've finally reached this stage in Python packaging! 🎉

---

_Comment by @bluss on 2024-08-21 14:59_

@prrao87 if you don't like to install pyproject-local-kernel ([or others](https://github.com/goerz/python-localvenv-kernel)), the following kernelspec here might be fun to install - however I share this for fun and learning, and I would actually recommend installing a dedicated python package instead of hacking around with kernelspecs..  (That said: the python package could be one with just this exact kernelspec! The reason you want to use packages is for reproducible environments, uv style!)

https://bluss.github.io/pyproject-local-kernel/FAQ/#isnt-there-a-less-complicated-way-to-do-it

---

_Referenced in [astral-sh/uv#6334](../../astral-sh/uv/issues/6334.md) on 2024-08-21 15:04_

---

_Comment by @lacker on 2024-09-04 22:23_

For others running into this thread, to get a uv environment working inside VSCode:

```
uv add ipykernel
uv run ipython kernel install --user --name=uv
```

Then - important step - restart VSCode. Then your jupyter notebooks will have a button at the upper right to select a kernel, choose "jupyter kernel", then "uv" will be one of the selections.

---

_Comment by @bluss on 2024-09-05 16:05_

@lacker If you don't mind, I would suggest the following for **VS Code and VSCodium** **specifically**:


```bash
# Create a new notebook project
# and open the directory in code
uv init my-notebook
cd my-notebook
uv add ipykernel
code .
```

- Now that the new project directory is open in code, use the action "Create: New Jupyter Notebook"
- Click Select Kernel -> Python Environments
- Select the virtual environment that uv created. It will be named `.venv/bin/python` in this dropdown (or maybe `.venv\Scripts\python` on windows)

![image](https://github.com/user-attachments/assets/22d80999-5394-4cab-98f5-8c67edf7631e)

Run this in the notebook to verify that the expected virtualenv is being used.

```python
import sys
sys.executable
```

It will respond with a path such as `/home/user/my-notebook/.venv/bin/python` or similar.


You can now run commands such as `!uv add pandas` etc in the notebook to add more dependencies to the project.

I prefer this way - without registering kernelspecs - because it's easier to work with, also collaboratively, and scales better to many notebook projects. Let me know what you think.
Code also supports using pure python formats such as py:percent for jupyter notebooks, which are another nice way to improve collaborative work, using git etc, with jupyter.


---

_Comment by @lacker on 2024-09-05 18:01_

In my situation I didn't want to open a new project just for this notebook, I had an existing (non-python) project that I wanted to run the notebook inside. But, I see that you can open from the venv directly rather than doing `uv run ipython kernel install`. You do have to restart VSCode to pick up the venv after you create the venv! (That's what I missed originally.) So, yeah I like your recommendation of opening from the venv better than creating the kernelspec.

---

_Comment by @araichev on 2024-09-10 01:20_

So is there a way yet to run a Jupyter notebook within the UV project's virtual environment from the command line (and without VS Code)?
I use Jupyter notebooks for data pipelines within my Python projects, and this is the one missing feature stopping me from switching from Poetry to UV.


---

_Comment by @zanieb on 2024-09-10 02:16_

@araichev How do you do it with Poetry?

---

_Comment by @araichev on 2024-09-10 02:51_

I don't actually do it with Poetry but with Poetry (to manage the project's dependencies) + Virtualenvwrapper (to manage project's virtual environment).
```
# At start of project
> mkdir PROJECT_DIR
> cd PROJECT_DIR
> mkvirtualenv -p3.11 PROJECT_VENV
> poetry add jupyter geopandas
> jupyter notebook

# Can now use GeoPandas inside notebook

# Revisiting project after start
> cd PROJECT_DIR
> workon PROJECT_VENV
> poetry add lets_plot
> jupyter notebook

# Can now use GeoPandas and Lets-Plot inside notebook
```
If UV could do this by itself without Virtualenvwrapper, then i'd be sold.


---

_Comment by @baggiponte on 2024-09-10 09:10_

> I don't actually do it with Poetry but with Poetry (to manage the project's dependencies) + Virtualenvwrapper (to manage project's virtual environment).
> 
> ```
> # At start of project
> > mkdir PROJECT_DIR
> > cd PROJECT_DIR
> > mkvirtualenv -p3.11 PROJECT_VENV
> > poetry add jupyter geopandas
> > jupyter notebook
> 
> # Can now use GeoPandas inside notebook
> 
> # Revisiting project after start
> > cd PROJECT_DIR
> > workon PROJECT_VENV
> > poetry add lets_plot
> > jupyter notebook
> 
> # Can now use GeoPandas and Lets-Plot inside notebook
> ```
> 
> If UV could do this by itself without Virtualenvwrapper, then i'd be sold.

Yes, I do this everytime. EDIT: you don't need virtualenvwrapper, but neither you do with poetry. If you do `poetry/uv add jupyter` you should just run `poetry/uv run jupyter` to run the jupyter in the venv.

The thread is about something different: running jupyter without having it installed in the venv.

---

_Comment by @araichev on 2024-09-10 22:08_

Oops, my bad. Thanks for the clarification @baggiponte . I wasn't able to get `uv run jupyter notebook` to work a few weeks ago, but today, after upgrading to UV version 0.4.8, it works. Hooray!

---

_Referenced in [astral-sh/uv#7625](../../astral-sh/uv/pulls/7625.md) on 2024-09-22 17:56_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-22 18:07_

---

_Label `documentation` added by @charliermarsh on 2024-09-22 18:07_

---

_Comment by @charliermarsh on 2024-09-22 18:16_

I am starting to explore and document this stuff here: https://github.com/astral-sh/uv/pull/7625

---

_Comment by @charliermarsh on 2024-09-22 18:25_

@bluss -- Do you know of any way to enable `!uv add` et al within Jupyter within VS Code, _without_ installing `ipykernel` into the project itself?

---

_Comment by @bluss on 2024-09-22 19:12_

@charliermarsh jupyterlab is my primary interface to jupyter, so I don't really know Code that well, especially not the best way to do things. VS Code experts please help.


----

Non-experimental: What do people do who are used to this? Just use the integrated terminal I guess, and initialize the project there.

And VS Code already offers to create a new environment when you want to select jupyter kernel. Would it be possible for them to put "Create new uv project" beside the venv and conda options?

![bild](https://github.com/user-attachments/assets/1a476711-5ffd-4667-ad0a-cf96e4d7dc5e)


----


Experimental stuff follows.. I would use this as starting point for a conversation, not as a recommendation because it's not a battle tested way to work.

Things get funky (good and bad) if one would install and use this jupyter kernelspec file -- see ["less complicated way to do it"](https://bluss.github.io/pyproject-local-kernel/FAQ/#isnt-there-a-less-complicated-way-to-do-it). It's installed into `.local/share/jupyter/kernels/uvrun/kernel.json`. (The correct way to distribute this file is probably via PyPI, in a distribution package.)

You can select this kernel in VS Code. (Select Another Kernel => Jupyter Kernel => Uv Run). However I don't know what triggers it to reload the kernelspec list and how to get Uv Run to reliably show up in the kernel list the *first time* after you add the file.

![bild](https://github.com/user-attachments/assets/a811523d-5b15-4bd1-a504-2d5565cbcea4)


If one uses this kernel in a newly created notebook in a new directory - no pyproject.toml's above this directory, uv will launch ipykernel in an emphemeral environment. Then `!uv init && uv add dependency` works. Then the user needs to restart the kernel ('Restart' button), and ipykernel execution will switch to the newly installed project. That's a load of magic :slightly_smiling_face: 


---

_Referenced in [astral-sh/uv#7679](../../astral-sh/uv/issues/7679.md) on 2024-09-25 06:02_

---

_Closed by @charliermarsh on 2024-09-26 00:55_

---

_Comment by @akupiectdc on 2025-01-30 16:31_

Hi,

I know this topic is closed but as I am about to migrate from Conda - just wanted to ask if it is possible to replicate my current workflow and create some 'draft' venvs, not necessary projects, that would allow to use these venvs in some research and exploration with the use of Jupyter notebooks in Jupyter tools and VS Code.

An example:

1. conda create --name sample python=3.11
2. conda activate sample
3. conda install conda-forge <packages>
4. conda install conda-forge::ipykernel
5. python -m ipykernel install --user --name sample --display-name "sample"

Of course one can use pip instead of Conda-forge channel.

After following these steps I can easily run notebooks with sample kernel both in Jupyter tools (f.e. Jupyter notebook command within activated venv) and the kernel is visible in VS Code in like any folder (I simply need to select the correct venv). 

Is it replicable with uv? If so - what are the recommended steps? 

If it should be a subject for another topic - I will create one, however I felt it might be somehow related to the OP requirements.

---

_Comment by @manzt on 2025-02-02 05:48_

It sounds like the [Using uv with Jupyter](https://docs.astral.sh/uv/guides/integration/jupyter/) guide likely covers your question.

---
