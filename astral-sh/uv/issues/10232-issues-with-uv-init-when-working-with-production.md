---
number: 10232
title: Issues with uv init when working with production/staging environments
type: issue
state: closed
author: elieobeid7
labels:
  - question
assignees: []
created_at: 2024-12-30T07:05:46Z
updated_at: 2025-01-01T13:56:09Z
url: https://github.com/astral-sh/uv/issues/10232
synced_at: 2026-01-10T01:24:51Z
---

# Issues with uv init when working with production/staging environments

---

_Issue opened by @elieobeid7 on 2024-12-30 07:05_

I have an existing project that uses pip and virtual env in a folder called `foo bar`. I want to migrate from pip to uv. 

If I do `uv init`, I get this error

```
error: Not a valid package or extra name: "foo bar". 
Names must start and end with a letter or digit 
and may only contain -, _, ., and alphanumeric characters.
```

On the surface, you'd say oh what's the fuss about? Just change the folder name already, except it's not so simple, you see on my PC sure, I can call it whatever you like, if you like dua-lipa, I'll call the project dua-lipa.

But on the server, I have two folders, a `foo bar staging` that uses the staging git branch, and a `foo bar prod` that uses the master git branch. and I have a whole CI/CD system and bash script to manage deployment.

If uv is going to complain about the folder name and take the folder name into consideration, we're off to a very bad start, it means I have to maintain two different dependencies files just because the name is different in staging and production and I have to fix git merges and ignore stuff from being merged, and I don't even know how to do all that on my local PC.

You see, on my  PC, I have only 1 folder, I can switch between staging and prod at will, whereas on the server I have two folders. So let's assume on my PC, I'm working on master branch and the package name matches the folder name, this will break when I switch to the staging branch, because it has a different package name that corresponds to the staging folder on the server.

I wish there is an option to just use uv as a package manager like pip or npm or PHP composer, and not a project manager that considers itself responsible for the entire folder as a project.


---

_Comment by @jromal on 2024-12-30 14:43_

Why would you do `uv init` in production? I would imagine `uv sync` or at least `uv add` and have the code and folders managed by a version management tool (git). I am not a professional programmer, and therefore I simplified too much but I see `uv init` only done once before the first commit. Then git manages the content. 

---

_Comment by @elieobeid7 on 2024-12-30 17:31_

@jromal I'm new to uv but it is my understanding that `uv init` creates the virtualenv used  by uv to manage dependencies, therefore I do the `uv init` to create the virtualenv needed to store the dependencies.

---

_Comment by @charliermarsh on 2024-12-30 17:33_

I think you're mixing up `uv init` with `uv venv`.

---

_Label `question` added by @charliermarsh on 2024-12-30 17:33_

---

_Comment by @elieobeid7 on 2024-12-30 18:35_

@charliermarsh I can't just run `uv venv` without `uv init` first, I get this error when I try to install a package via `uv add`

```
error: No `pyproject.toml` found in current directory or any parent directory
```

And `pyproject.toml` contains the project name that `uv init` complains about.

Should I create the `pyproject.toml` manually and use any random project name? Would that work?


---

_Comment by @jromal on 2024-12-30 21:33_

I think you are a bit confused. 

`uv init` it is for project initialization. For creating a new project. But you have already a project that you deploy already. 

I have changed a couple of projects from pip to uv in this way.
* Create a "pythonproject.toml", either manually or any other form (like `uv init`)
* Preferably a ".python version" file too.
* Add the requirements to pythonproject file, manually or with `uv add`. 
* On the deployment pipeline, install "uv" and call `uv sync` instead of all pip commands.

This is how, a hobby programmer, does it. There are other ways to do it, like using `uv add` or `uv pip install`, but in my opinion, `uv sync` is the best option. 


---

_Comment by @zanieb on 2024-12-30 21:55_

It sounds like perhaps `uv init` should normalize the working directory name when attempting to construct a project name? However, it sounds like that's not what you want.

> I wish there is an option to just use uv as a package manager like pip or npm or PHP composer, and not a project manager that considers itself responsible for the entire folder as a project.

There is — `uv pip`. The top-level uv commands are generally for project management. `uv add` requires a `pyproject.toml`. The overview at https://docs.astral.sh/uv/getting-started/features/ may be helpful.

---

_Referenced in [astral-sh/uv#10245](../../astral-sh/uv/issues/10245.md) on 2024-12-30 21:56_

---

_Comment by @elieobeid7 on 2024-12-31 07:48_

@zanieb you're saying that `uv init` should normalize the working directory, yes that seems like a good solution, npm and composer do that already, so it's been tested in production and that works

`uv pip` seems interesting, why use it instead of pip? what's the selling point? the speed of package installation?

---

_Comment by @elieobeid7 on 2024-12-31 07:55_

@jromal you're saying that `uv init` is not needed, but again you're not answering the main question here, I'm not familiar with how `uv` works under the hood

there's the issue that `pyproject.toml` requires a project name for it to work. ok fine I can create that manually, but then if I have a parent folder called `staging` and another parent folder called `production`, they're the same project mind you, just a staging version and a production version. Should the `pyproject.toml` project name match the parent folder name? if that's the case then it's a problem because one would get merge conflicts when merging production and staging, one would have to git ignore the `pyproject.toml` during merges and update it manually, that's very inconvenient.

On the other hand if the project name in `pyproject.toml` is meaningless for `uv` and it's just there for the sake of being there, much like npm or composer, doesn't affect anything, then sure I could create the `pyproject.toml` manually.

---

_Comment by @jromal on 2024-12-31 12:38_

As I said, I am just a regular user trying to help. @zanieb and @charliermarsh are part of the UV team.

 `uv` has the concept of workspaces, where you can decide if those folders should be part of the same folder. Check the docu. 

---

_Comment by @charliermarsh on 2024-12-31 15:22_

> there's the issue that pyproject.toml requires a project name for it to work. ok fine I can create that manually, but then if I have a parent folder called staging and another parent folder called production, they're the same project mind you, just a staging version and a production version. Should the pyproject.toml project name match the parent folder name? if that's the case then it's a problem because one would get merge conflicts when merging production and staging, one would have to git ignore the pyproject.toml during merges and update it manually, that's very inconvenient.

I don't fully understand the setup you're describing (the same project, duplicated under two different folders?), but no, the project name doesn't have to match the parent folder name. We do that by default, but `uv init` accepts a `--name` flag.


---

_Comment by @elieobeid7 on 2024-12-31 16:36_

@charliermarsh 

> I don't fully understand the setup you're describing (the same project, duplicated under two different folders?), 

Let's take this example, so let's say you host astral.sh on an ec2 instance, it uses fast API behind NGINX. and the main git branch is `master`, and the folder containing the git project is called `astral`

Now let's say you want to redesign it, what would you do in that case? you'd create a git branch called `staging`, but you also need to host this branch on the server, because the designers need to see it and work on it.

You would clone the same repo on the server again inside a folder called `dev.astral` and then you checkout the `staging` git branch. and set nginx to serve it under the subdomain `dev.astral.sh`

What just happened? On your PC, you have one git folder, containing a master branch and a staging branch, but on your server you have 2 folders, you cloned the same repo twice, once for astral.sh and once for dev.astral.sh

This is the usecase I'm working with.

---

_Comment by @jromal on 2024-12-31 17:56_

What would you do with your virtual environments not using uv? 

Do the same with uv. 

If you checkout for a branch you work on that branch. This includes `pyproject.toml`. Git will have the right development state. When you change branch you do `uv sync` to refresh the virtual environment, that is excluded on `.gitognore`. 

Or you work in two completely separated folders like they would be two projects. And each folder has all project files. 

Why don't you try it? It is very simple. 

---

_Comment by @zanieb on 2024-12-31 21:33_

It sounds like you should either

1. `uv init` and commit the `pyproject.toml` to both your production and staging branches, then proceed as normal with `uv sync`

or

2. Use `uv pip` (or plain `pip`, if you don't want to use uv) with your existing setup.

The benefits of using `uv pip` over pip are roughly:

- Can be installed outside the environment
- Better resolver error messages
- Better performance
- More advanced features

---

_Comment by @elieobeid7 on 2025-01-01 11:02_

> What would you do with your virtual environments not using uv?

I use pip and `requirements.txt`, not `pyproject.toml`, since `requirements.txt` is just a text file that store dependencies without any project name and whatnot, I don't have all those problems discussed here.

---
Ok will try the suggestions mentioned by @zanieb and @jromal 

---

_Comment by @charliermarsh on 2025-01-01 12:50_

> I use pip and requirements.txt, not not pyproject.toml, since requirements.txt is just a text file that store dependencies without any project name and whatnot, I don't have all those problems discussed here.

Got it. Whatever you do here, you can do with `uv pip` instead of `pip`. Alternatively, you can use `uv init` and `uv sync` -- there is _no_ requirement that the directory name matches the project name, and it's not enforced on the CLI or anything like that. (I'm going to close this for now as I don't see anything actionable here for us.)


---

_Closed by @charliermarsh on 2025-01-01 12:50_

---
