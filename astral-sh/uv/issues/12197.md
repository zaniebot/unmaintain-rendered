```yaml
number: 12197
title: Improve devcontainer experience, particularly when switching from pip to uv.
type: issue
state: open
author: jtkiley
labels:
  - enhancement
assignees: []
created_at: 2025-03-15T21:06:14Z
updated_at: 2025-09-16T20:30:45Z
url: https://github.com/astral-sh/uv/issues/12197
synced_at: 2026-01-10T03:23:53Z
```

# Improve devcontainer experience, particularly when switching from pip to uv.

---

_Issue opened by @jtkiley on 2025-03-15 21:06_

### Summary

I tried changing an existing devcontainer to use `uv` instead of `pip`, and I ran into the error mentioned in ##11599.

Initial devcontainer.json with only uv added:

```
{
	"name": "Example",
	"image": "mcr.microsoft.com/devcontainers/python:1-3.12-bookworm",
	"features": {
		"ghcr.io/devcontainers-extra/features/apt-get-packages:1": {},
		"ghcr.io/va-h/devcontainers-features/uv:1": {}
	},
	"postCreateCommand": "pip install -r .devcontainer/requirements.txt",
	"customizations": {
		"vscode": {
			"extensions": [
				"ms-toolsai.jupyter",
				"charliermarsh.ruff"
			]
		}
	}
}

```

requirements.txt: 

```
chromadb==0.6.3
ipykernel==6.29.5
ipywidgets==8.1.5
lancedb==0.21.1
polars==1.24.0
sentence-transformers==3.4.1

```

When I change `postCreateCommand` to use uv like this `"postCreateCommand": "uv pip install -r .devcontainer/requirements.txt"`, I get this:

```
Running the postCreateCommand from devcontainer.json...

[4474 ms] Start: Run in container: /bin/sh -c uv pip install -r .devcontainer/requirements.txt
error: No virtual environment found; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
```

When I then add `--system` to get `"postCreateCommand": "uv pip install --system -r .devcontainer/requirements.txt"`, I get another error:

```
      Built pypika==0.48.9
Prepared 134 packages in 8.74s
error: Failed to install: websockets-15.0.1-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (websockets==15.0.1)
  Caused by: failed to create directory `/usr/local/lib/python3.12/site-packages/websockets-15.0.1.dist-info`: Permission denied (os error 13)

```

### Example

Ideally, a user could simply add a `uv` feature to a devcontainer, change the `postCreateCommand` to use `uv pip` instead of `pip`, and have it work.

More broadly, it would be nice to see worked examples of how uv works nicely with devcontainers. Among other things, I work with beginners (other PhD researchers), and I build a devcontainer for the course I teach (and use them extensively in my own projects, both academic and external). It would be nice to see uv work well throughout the lifecycle of a project:

1. Creation. If I'm creating a new devcontainer (e.g., using VSCode "Dev Containers: Add Dev Container Configuration Files..."), how can I do so easily? I usually build the container, `pip install` the needed packages, let it solve, `pip freeze`, and use that output to pin only the versions of the key packages (intended to be a portability habit). Then I rebuild again and work.
2. Adding uv to an existing project. Ideally, I could do what I described above, but what's the intended way to do this? I tend to think that it should drop in a replacement for pip and then let folks work from there to use the other features.
3. Adding packages. How should we add new packages? (This seems covered in the normal docs.)
4. Updating packages. Academic projects often last for years, so there are often package updates that would add something helpful. How can I update packages, test if anything is broken (more like ad hoc experimentation than `pytest`), and keep/rollback the update? I usually teach a `pip freeze` approach, but the docs make it look potentially much easier with uv.

I think it would be great if uv had a devcontainer feature that simply handled all of the venv, `--target`, and other differences for users. In a devcontainer, there's generally just one Python install that matters, and that's `usr/local/bin/python`, which I think is the root of a lot of the previous discussion about `--user`.

---

_Label `enhancement` added by @jtkiley on 2025-03-15 21:06_

---

_Comment by @juanjcardona13 on 2025-03-16 00:45_

Same for here!
What I can see is that the interpreter inside the venv disappears. I'm not sure if it's related to it being a symbolic link; I'm pretty unfamiliar with the topic, but that's what I noticed.
Thanks

---

_Comment by @zanieb on 2025-03-16 01:02_

@juanjcardona13 could you please provide more details? that sounds like a different thing?

---

_Comment by @zanieb on 2025-03-16 01:16_

First of all, we can definitely create an integration guide for this. Thanks for flagging some of the things you'd want to see there.

I gave your example a quick try, and `"postCreateCommand": "uv venv && uv pip install anyio",` works fine. Why not just create the virtual environment?

If you're opposed to using a virtual environment since it adds indirection, then you can change to the root user `"remoteUser": "root",` and the permission error is also resolved.

Generally, I'd recommend using our top-level interface, e.g., `uv add`, `uv sync`, and `uv run`. The workflows you're asking about are much simpler that way and management of the environment is abstracted away.

---

_Comment by @jtkiley on 2025-03-16 17:15_

I think it would be great to have a resource to link for people. As availability allows, I'm happy to help draft a bit and/or test.

I edited that line to `"postCreateCommand": "uv venv && uv pip install -r .devcontainer/requirements.txt"`, rebuilt the devcontainer, and that worked for the setup with some caveats.


1. **Hardlink warning.** In the rebuild, I got the warning below.

```
[0/135] Installing wheels...                               warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 135 packages in 12.89s
```
2. **Existing notebook.** An existing notebook already has a kernel chosen, and it's `usr/local/bin/python`. In an existing notebook, I get the error below. This is tough, because that package is installed, but now it's in a venv. Also, this is a notebook that worked perfectly before adding uv to the container. Following the suggestion in the error message won't really help, because it'll just move the missing package issue down the line until the imports in the notebook fail. This is an issue with requiring venvs.
```
Running cells with 'Python 3.12.1' requires the ipykernel package.
Run the following command to install 'ipykernel' into the Python environment. 
Command: '/usr/local/bin/python -m pip install ipykernel -U --user --force-reinstall'
```
3. **First new notebook.** When creating a first new notebook and attempting to run a code cell, the only recommended Python kernel is `/usr/local/bin/python`. The venv does appear in other environments.
4. **Subsequent new notebooks.** When creating another new notebook after having set the venv as the environment for the previous new notebook, both the venv (listed first) and `/usr/local/bin/python` (second) are listed without either shown as recommended.
5. **Terminal.** In the terminal (within VSCode and running in the devcontainer), `which python` is `/usr/local/bin/python`.
6. **.py files.** If I create a `.py` file, and then click "Run" in VSCode, it attempts to run it with `/usr/local/bin/python`.

I think things like this are why some devcontainer users really dislike the required venv.
It's not clear that it adds value to have it in a devcontainer, and it creates these foreseeable problems. Many of us remember exactly these issues from running locally with things like conda. After switching to devcontainers (conda friction and issues being a big motivator, at least for me), we're used to never seeing these at all. You set it up, dial it in, and it just works, and it works everywhere. That I think is what the ideal target is: a straightforward configuration that just works, and with as much as possible included in the devcontainer feature, rather than being left for every downstream user to configure.

There's also the beginner/student case, which is more specific to some of what I do, but it also got some discussion in the other threads. I used to use conda, and I had to set aside an hour in a workshop to troubleshoot, and, in bad cases, troubleshoot during breaks or after hours. When I switched to devcontainers (and recommended using Codespaces), that dropped basically to zero.

Personally, I don't like the root idea. It runs against [this VSCode documentation suggestion](https://code.visualstudio.com/remote/advancedcontainers/add-nonroot-user), and I'm also not sure I can fully identify and mitigate the security implications for me and for wherever a devcontainer might end up (other devs, students, clients).

I'm definitely interested in the top level interface for new projects and modifying projects where uv has been added. At the moment, I'm just more focused on the switching case. I suspect that I'd rather not spend a few years with both setups, rather than spending what I hope is a small amount of time migrating everything to uv once the problem space and procedure become clear.

Edit: Forgot that Github doesn't ignore newlines in markdown paragraphs.

---

_Comment by @zanieb on 2025-03-16 18:05_

For most of those problems, it just sounds like you need to put the virtual environment at the front of your `PATH` so it is used by default? https://docs.astral.sh/uv/guides/integration/docker/#using-the-environment

> It runs against [this VSCode documentation suggestion](https://code.visualstudio.com/remote/advancedcontainers/add-nonroot-user), and I'm also not sure I can fully identify and mitigate the security implications for me and for wherever a devcontainer might end up (other devs, students, clients).

I actually don't see a recommendation not to use the root user there? It just explains how to do so. I agree it's nice not to use the root user, but.. you're also trying to write to global state which the author of the images have decided should require root permissions.

It looks like just using `sudo` in the invocation works for that image

> `"postCreateCommand": "sudo uv pip install anyio --system"`





---

_Comment by @zanieb on 2025-03-16 18:35_

Here's a more complete example using a virtual environment

```
{
	"name": "Python 3",
	"image": "mcr.microsoft.com/devcontainers/python:1-3.12-bullseye",
	"features": {
		"ghcr.io/va-h/devcontainers-features/uv:1": {}
	},
	"postCreateCommand": {
		// Fix permissions on the volume mount; see https://github.com/microsoft/vscode-remote-release/issues/9931
		// Create a virtual environment
		// Install an example package (you would install a requirements file in practice)
		"setupEnvironment": "sudo chown -R $(whoami): ${containerWorkspaceFolder}/.venv && uv venv ${containerWorkspaceFolder}/.venv && uv pip install anyio"
	},
	"mounts": [
		// Mount the `.venv` as a volume to avoid collision with the one  from
		// the host machine's workspace.
		// See https://github.com/astral-sh/uv-docker-example/blob/main/run.sh#L11
		"target=${containerWorkspaceFolder}/.venv,type=volume"
	],
	"remoteEnv": {
		// Activate the virtual environment
		"PATH": "${containerWorkspaceFolder}/.venv/bin:${containerEnv:PATH}"
	},
	"customizations": {
		// Use the virtual environment interpreter by default
		"vscode": {
			"extensions": [
				"ms-python.python",
				"ms-python.vscode-pylance"
			],
			"settings": {
				"python.defaultInterpreterPath": "${containerWorkspaceFolder}/.venv/bin/python"
			}
		}
	}
}
```

---

_Comment by @jtkiley on 2025-03-16 18:56_

(Started writing this before the most recent replies.)

I spotted another issue that seems to be a venv issue.

After I wrote the previous part up, I noticed that Onedrive was syncing a lot of data. And, I could see in the filenames that it looked like a lot of package internals. So, I created a second copy of the devcontainer configuration and the notebook/data I'm working with.

- pip: 1.4MB
- uv: 1.39GB

It appears that the stock Python devcontainer image uses caching to store a lot of the devcontainer packages, so none of that hits the workspace that is linked into the container from the host filesystem.

In contrast, the uv venv is all in the `.venv/` folder in the workspace, which ends up getting synced in full. In hindsight that's not surprising, since there's nothing linking up uv with that caching.

I poked around and didn't quite see explicitly how the caching works, but it may use the vscode docker volume that's mounted across containers. The top of the rabbit hole I went down is the [Python image source](https://github.com/devcontainers/images/tree/main/src/python). There are some environment variables in the container that may be relevant, but I don't know enough about those internals to be sure.


---

_Comment by @jtkiley on 2025-03-16 19:01_

> > It runs against [this VSCode documentation suggestion](https://code.visualstudio.com/remote/advancedcontainers/add-nonroot-user), and I'm also not sure I can fully identify and mitigate the security implications for me and for wherever a devcontainer might end up (other devs, students, clients).
> 
> I actually don't see a recommendation not to use the root user there? It just explains how to do so. I agree it's nice not to use the root user, but.. you're also trying to write to global state which the author of the images have decided should require root permissions.
> 
> It looks like just using `sudo` in the invocation works for that image
> 
> > `"postCreateCommand": "sudo uv pip install anyio --system"`

Here's a link to [the section](https://code.visualstudio.com/remote/advancedcontainers/add-nonroot-user#_creating-a-nonroot-user) that says:

>Running your application as a non-root user is recommended even in production (since it is more secure), so this is a good idea even if you're reusing an existing Dockerfile.

For now, I wanted to point that out, and I'll test out the other things you posted. Thanks again for all of your help.


---

_Comment by @zanieb on 2025-03-16 22:21_

I agree using a non-root user in production makes sense. I'm not sure if I'd go out of my way to do so in a development container. Regardless, we should support it.

> It appears that the stock Python devcontainer image uses caching to store a lot of the devcontainer packages, so none of that hits the workspace that is linked into the container from the host filesystem.

Interesting... we'll need to look into that. I wonder if that's why we get a link-mode warning. I'm sure we can setup a shared uv cache.

---

_Comment by @jtkiley on 2025-03-24 15:52_

It's probably going to take a bit for me to circle back to more experimentation on this, but I did run across something adjacent that appears interesting.

It looks like Microsoft is developing an extension ([currently experimental](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-python-envs)) to handle environments and package management, with the idea being that it will provide a GUI and infrastructure for other extensions that can add support for their tools. In particular, it appears that some built in functionality uses uv if available, and that they're expecting a community extension beyond that (see https://github.com/microsoft/vscode-python-environments/issues/79).

I wonder if this will eventually help clean up at least some the new devcontainer friction. On top of that, a GUI for packages (especially if it's modifying configuration to keep sync between the state of the devcontainer and what a rebuild would produce) would be a big quality of life improvement for the new users I work with (see also, right click-"Add to devcontainer.json" for VSCode extensions). A common question that I answer with an informal demo is how to take the workshop devcontainer/requirements and modify them for their own projects.

---

_Comment by @jtkiley on 2025-08-26 15:18_

I had a chance to circle back to this, and I now have a prototype to try out with real work.

Overall, this feels like real progress from where I was in March. But, as the length of this post may imply (I've been chipping away at it for a bit), it's nuanced. I'm happy to get more suggestions or talk through the choices I settled on.


# uv

`devcontainer.json`:

```jsonc
{
	"name": "UV system test",
	"image": "mcr.microsoft.com/devcontainers/python:3.13-bookworm",
	"features": {
		"ghcr.io/rocker-org/devcontainer-features/quarto-cli:1": {
			"version": "prerelease"
		},
		"ghcr.io/jsburckhardt/devcontainer-features/uv:1": {}
	},
	"customizations": {
		"vscode": {
			"extensions": [
				"quarto.quarto",
				"ms-toolsai.jupyter",
				"ms-toolsai.vscode-jupyter-powertoys",
				"ms-python.mypy-type-checker",
				"mechatroner.rainbow-csv",
				"charliermarsh.ruff",
				"Posit.shiny",
				"myriad-dreamin.tinymist",
				"ms-toolsai.datawrangler",
				"tamasfe.even-better-toml"
			]
		}
	},
	"mounts": [
		"target=${containerWorkspaceFolder}/.venv,type=volume"
	],
	"containerEnv": {
		"UV_LINK_MODE": "copy",
		"UV_SYSTEM_PYTHON": "1"
	},
	"postCreateCommand": {
		"setupEnvironment": "sudo chown -R $(whoami): ${containerWorkspaceFolder}/.venv"
	},
	"postStartCommand": "[ -f pyproject.toml ] && uv sync || echo '    No pyproject.toml found, skipping uv sync.\n    If this is a new workspace, run uv init.'"
}

```

Several notes:

1. This is a start toward my own workflow, rather than being minimal. The goal is that, this `devcontainer.json` with nothing else produces a working, clean-sheet devcontainer. It's mostly a base for a more batteries-included template repository, but sometimes clean starts are desirable.
2. Quarto is nice for this kind of test, because it executes Python code, and having it just work is highly preferred.
3. I don't love the amount of boilerplate needed for `venv`s, particularly because they don't add value beyond the container's isolation. I should note that it's very bad/broken with the feature but without these.
4. The `postStartCommand` isn't technically required, but you'll have no packages and your stuff won't work without it if you rebuild the container. In data science/research workflows, I rebuild containers all the time. uv, via the lock file, guarantees Python packages in a way that pip/requirements.txt does not. However, it's not uncommon to have apt-installed system packages, intermediate computations, or other things where a rebuild can demonstrate that the config reproduces the results.
5. The mostly good news is that those `venv` workarounds *should* (😬) be able to go into a uv `devcontainer-feature.json`. That would clean up a lot about the user experience. One way or another, I think a Python devcontainer image with the uv feature added and nothing more should just work. That probably entails some thought about use cases.
6. It's implicit, but I'm depending on the Python Environments plugin (v1.2.0 is what's current for me in stable). It sources the `venv` in every terminal, including those for previews, without clobbering my dotfiles. It also seems to help generate better recommendations in Jupyter notebooks. On the other hand, it allows for GUI package install, but it uses `pip`, so they disappear on `uv sync`.
7. uv having a huge number of options configurable via environment variable is so nice for this use case. I didn't use that many in the end, but I experimented with several.


# An alternative: poetry

I also built a version with poetry. It's a mixed story.


`devcontainer.json`:

```jsonc
{
	"name": "poetry test",
	"image": "mcr.microsoft.com/devcontainers/python:3.13-bookworm",
	"features": {
		"ghcr.io/rocker-org/devcontainer-features/quarto-cli:1": {
			"version": "prerelease"
		},
		"ghcr.io/devcontainers-extra/features/poetry:2": {}
	},
	"customizations": {
		"vscode": {
			"extensions": [
				"quarto.quarto",
				"ms-toolsai.jupyter",
				"ms-toolsai.vscode-jupyter-powertoys",
				"ms-python.mypy-type-checker",
				"mechatroner.rainbow-csv",
				"charliermarsh.ruff",
				"Posit.shiny",
				"myriad-dreamin.tinymist",
				"ms-toolsai.datawrangler",
				"tamasfe.even-better-toml"
			]
		}
	},
	"containerEnv": {
		"POETRY_VIRTUALENVS_CREATE": "false"
	},
	"postCreateCommand": "[ -f pyproject.toml ] && poetry install --no-root || echo '    No pyproject.toml found, skipping poetry install.\n    If this is a new workspace, run poetry init.\n    If this is not a package, you may want to add the following to pyproject.toml:\n\n    [tool.poetry]\n    package-mode = false'"
}

```

1. Right away, getting rid of `venv`s was huge. I spent several hours on the uv version, and this was working in 15 minutes. Almost all of that difference was figuring out `venv` issues.
2. However, I generally like using uv better than poetry.
3. The user experience snag here is that `package-mode = false` cannot be set by environment variable. Without that, there are errors, first because it's empty and later because data science work often isn't a package. (will file an issue there)
4. The permissions for `site-packages` issue cleanly falls back to PEP370. There are spammy file overwrite notices, but it just works.


# What I did/did not test (yet)

1. I started with a fresh devcontainer with the features (uv or poetry in those variants).
2. Then I iterated until they worked, and I used a `.py` file, `.ipynb`, and `.qmd` (embedded python code to run and typst output) to help test that.
3. Along the way, I did (many) rebuilds and occasionally deleted the container altogether to recreate.
4. Once working, I took the `devcontainer.json` file only, moved it to another folder, opened it in VS Code, and let it build the container.
5. There, I cleaned up the cases with no `pyprofile.toml` and similar clean start issues.
6. I have not yet tried retrofitting this into an existing project. I suspect that I'd try moving from requirements.txt to pyproject.toml in one step (still using pip) and then move from pip to uv in another step, at least when validating it. It might be as easy as `uv init && uv add -r requirements.txt`, remove the pip install and requirements.txt file, and rebuild the container.


# Ideas for improvement

1. `venv`s. Clearly, a lot of us don't like them in this context, and there's been a lot of discussion, and it seems like the policy is what it is. That said, it looks like they can be worked around. I think a lot of the frustration results from uv having a goal of being the best tool unconditionally (I like that goal) and yet this use case is constrained by the `venv` policy without the benefits (I totally get it in non-container cases). Under the circumstances, it seems like making it effectively transparent in devcontainers is a good way forward, if potentially tricky.
2. Python Environments extension. It looks like this is on its way to being a notable quality of life improvement, and perhaps it signals that Microsoft would like tooling to make the `venv` issue better. That may include the new user experience, GUI package install (with a coordinating uv extension fixing the disappearing package problem), and auto sourcing in terminals (that works now).
3. Fresh devcontainer use case. If I open VS Code, Cmd-Shift-P, add devcontainer configuration files, choose the Python image, and add the uv feature, where do I go from there once the container is built? I'm using that post start check to provide a direction via terminal message. A better alternative may be if uv (via a command) can detect this case (in a devcontainer that doesn't have pyproject.toml) and set an environment variable, otherwise `uv sync`. Then, the uv extension checks that environment variable (or other communication method) and launches a VS Code extension walkthrough. Alternatives could be generating a markdown file and/or providing a web guide. It's different enough from the standard "getting started" use case.
4. Caching. I didn't get very deep into this, but it's noticable that uv downloads everything on every rebuild (appears that poetry does, too), and pip does not.


---

_Comment by @jtkiley on 2025-09-13 17:20_

> 6\. It's implicit, but I'm depending on the Python Environments plugin (v1.2.0 is what's current for me in stable). It sources the `venv` in every terminal, including those for previews, without clobbering my dotfiles. It also seems to help generate better recommendations in Jupyter notebooks. On the other hand, it allows for GUI package install, but it uses `pip`, so they disappear on `uv sync`.

This is still true in v1.6.0 of the extension. I think it will be until a `uv` extension enables it to use the native `uv` interface, which will automatically do the `pyproject.toml` and `uv.lock` updates.

---

_Comment by @schlich on 2025-09-13 22:17_

I'm curious about the volume mount you made explicit with the .venv folder.  Doesn't the project root get bind mounted automatically? Or is "volume" mapping different?

I ask bc I used to do similar mounting but can't remember why I started/stopped.  Usually the default is good enough for caching though since uv puts them in the project folder

---

_Comment by @jtkiley on 2025-09-16 20:30_

> I'm curious about the volume mount you made explicit with the .venv folder. Doesn't the project root get bind mounted automatically? Or is "volume" mapping different?
> 
> I ask bc I used to do similar mounting but can't remember why I started/stopped. Usually the default is good enough for caching though since uv puts them in the project folder

Hey, good question. As I understand it, the problem is that the `.venv/` folder on the host is going to be platform specific and may have different paths to things, and using both the host and container will have them clobbering each other. Even if you don't use the host (I don't), you can't guarantee that others won't if it's more than a solo project.

I got it (and the `chown` for it) from @zanieb's example https://github.com/astral-sh/uv/issues/12197#issuecomment-2727583853. And, the rationale is covered in the [uv documentation](https://docs.astral.sh/uv/guides/integration/docker/#developing-in-a-container).

It's a little weird in that the mount creates an empty folder on the host, and it'll complain if you try to delete it while a container is running, and the folder will come back anyway. It's seemingly harmless, though.

What I'm not sure about is whether this should always be done in containers/devcontainers. I suspect the answer is yes, though. If so, it would be nice to push the `.venv` volume setup, `chown`, and `UV_LINK_MODE=copy` into the uv devcontainer feature to eliminate the boilerplate for users.

I can test it a bit and make a PR for those changes on the devcontainer feature if it's a good idea and I'm not missing something.

---
