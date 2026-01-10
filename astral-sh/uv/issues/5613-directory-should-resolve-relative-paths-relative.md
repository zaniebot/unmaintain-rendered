---
number: 5613
title: "`--directory` should resolve relative paths relative to current working directory"
type: issue
state: closed
author: charliermarsh
labels:
  - cli
assignees: []
created_at: 2024-07-30T17:24:05Z
updated_at: 2024-10-09T09:05:53Z
url: https://github.com/astral-sh/uv/issues/5613
synced_at: 2026-01-10T01:23:50Z
---

# `--directory` should resolve relative paths relative to current working directory

---

_Issue opened by @charliermarsh on 2024-07-30 17:24_

_No description provided._

---

_Label `cli` added by @charliermarsh on 2024-07-30 17:24_

---

_Comment by @bluss on 2024-07-30 18:02_

Uv run's external command is opaque (in meaning), so it's not possible to translate those arguments. One part of the solution for uv run could be either to never change current directory, or set cwd back to the original for the child process.

---

_Comment by @charliermarsh on 2024-07-30 20:07_

I was thinking that we’d remove the current set_directory call, and then pass around a project directory as dictated by the directory flag.

---

_Referenced in [astral-sh/uv#6733](../../astral-sh/uv/issues/6733.md) on 2024-08-28 13:28_

---

_Comment by @abdok96 on 2024-09-12 23:27_

Do you have any timeline for this change by any chance? I'm currently using the hidden `--directory` option but it would be great if it can be made public (and ideally add an environment variable for it). Also resolving the paths relative to the passed directory is causing very weird issues in different places. I think this change is the correct choice.

One example where paths relative to the current directory are necessary is when running uv commands from pre-commit local hooks, paths of the files staged for commit are automatically passed to the hook entry (uv command in this case) and the user cannot control this behavior. 
 
Thanks for uv it is looking great! 

---

_Comment by @zanieb on 2024-09-13 01:57_

per https://github.com/astral-sh/uv/issues/6733#issuecomment-2315983710 our existing behavior matches the Git and Cargo CLIs so we're very unlikely to change it.

The pre-commit example is great though. How do people work around this in other tools? Why do you need to use `--directory` in this situation? Maybe we need an environment variable to toggle.

---

_Comment by @abdok96 on 2024-09-13 02:32_

Thanks for the quick reply. 
In our case, the simplified project structure was briefly something like:

my-project/
|__ app/
|    |__ main.py
|    |__ pyproject.toml  
|__ docker/
|__ docs/
|__ bin/
|__ docker-compose.yml
|__ .pre-commit.config.yaml

We also have a local hook for mypy something like:

```
  - repo: local
    hooks:
      - id: mypy
        name: run mypy check
        language: system
        entry: uv run --directory app -- mypy
        types_or: [python, pyi]
        require_serial: true
        args:
          - --config-file=pyproject.toml
```

We want to run the mypy command from the virtual environment that has all the python dependencies so that mypy can resolve and follow all the imports when running the type checks.

Because of the behavior of `--directory` we are passing `--config-file` as `pyproject.toml` instead of `app/pyproject.toml` but that's okay since we can control it. 

However, when the commit hook is triggered by pre-commit, we have less control.
Let's say `app/main.py` was changed, the command executed by pre-commit will be something like:
`uv run --directory app -- mypy --config-file=pyproject.toml app/main.py`

And since the paths are resolved relative to the `--directory` param, mypy won't find a module matching `app/app/main.py`.

In our case, we managed to get around it by moving the `pyproject.toml` to the root directory which is probably a more suitable place.  However, there might be similar cases that are a bit harder to avoid.

---

_Comment by @abdok96 on 2024-09-13 02:50_

Btw, I'm not sure if it helps, but we are in the process of migrating from poetry to uv. I can confirm that in the case of poetry, the behavior is not similar to cargo or git. It resolves paths based on the working directory.  

For example, this is the same hook I mentioned in my previous comment, before the migration to uv:

```
  - repo: local
    hooks:
      - id: mypy
        name: run mypy check
        language: system
        entry: poetry --directory app run mypy
        types_or: [python, pyi]
        require_serial: true
        args:
          - --config-file=app/pyproject.toml
```

Notice how despite the `--directory` param, we used to pass the `--config-file` relative to the working directory not `/app`.
I guess both options have good arguments. A toggle like you suggested might help. 

---

_Comment by @bluss on 2024-09-13 06:36_

@zanieb Unlikely to implement this issue? What do you think about adding a separate flag to uv run with the behaviour intended by this issue?

---

_Comment by @bluss on 2024-09-13 07:14_

If we again look at git it has git -C x --git-dir y and maybe this could be a good model.

Uv can have one flag that changes directory, like -C and one flag that points out which project or pyproject.toml to discover and use as starting point. For convenience, this second project flag should also take a directory value.

Of course in the normal case these are linked and can be used for similar tasks.

---

_Comment by @aivarannamaa on 2024-09-13 22:44_

Expanding on the comment from bluss -- What about adding `--project`, which doesn't change the current directory, but simply indicates where to pick up the environment (or its specification)?

---

_Comment by @aivarannamaa on 2024-09-13 23:31_

One more idea: if `--project` references a script with inline metadata, this script would be used as the spec for the enironment to use.

For example:

* `uv python --project my_script_with_metadata.py find` would create and report the ephemeral interpreter which would be used for running this script
* `uv tree --project my_script_with_metadata.py` would list the packages in this ephemeral env. This would a good alternative for `uv tree --script my_script.py`, which I proposed under #7328

I would use both of these forms for adding `uv` mode to [Thonny IDE](https://thonny.org/).

---

_Comment by @charliermarsh on 2024-09-14 22:15_

I am open to a dedicated `--project` flag on the `uv run`, `uv sync`, `uv lock`, etc. to inform workspace discovery. \cc @zanieb 

---

_Comment by @dragly on 2024-09-18 09:38_

We are also very interested in a `--project` argument where the paths can remain relative to the current working directory. Our use-case is a monorepo where we are unable to use the workspace feature of `uv` due to conflicting dependencies between projects. This is the exact use of workspaces that is discouraged in the `uv` docs today.

(Another solution for us would be if `uv` had a feature where one could define a workspace without a shared `uv.lock`, but I think the `--project` argument sounds like a general solution that could solve more issues overall).

---

_Comment by @bluss on 2024-09-18 18:54_

When I take a first look at this, I think `--project` in `uv run --project` maybe needs to be a global argument as well? (Or run-specific but handled early.) Because this logic is global (happens before any subcommand) and it is about project/workspace discovery. By the simplest logic: currently it's assumed project root is CWD (or found from CWD), the updated code needs to start with the user provided project directory instead of CWD here.

https://github.com/astral-sh/uv/blob/e36cc99b0d17e38be2c6fb09f02eb7489536176c/crates/uv/src/lib.rs#L103-L129

---

_Comment by @zanieb on 2024-09-18 19:01_

I'm also willing to have `--project` — though Charlie said it'd be a pain to add support :)

---

_Comment by @charliermarsh on 2024-09-18 19:52_

I think project is pretty straightforward since it’s limited in scope as to what it affects. I assume it affects project discovery and maybe python-version?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-20 19:42_

---

_Comment by @charliermarsh on 2024-09-20 20:03_

I'll do it.

---

_Comment by @zanieb on 2024-09-20 20:04_

Epic. Sounds good to me. The `python-version` file stuff might be easier with https://github.com/astral-sh/uv/pull/6370

---

_Referenced in [astral-sh/uv#7603](../../astral-sh/uv/pulls/7603.md) on 2024-09-20 21:08_

---

_Closed by @charliermarsh on 2024-09-21 20:19_

---

_Comment by @dragly on 2024-10-09 08:50_

Thanks for implementing this! It works perfectly for our needs :)

---

_Comment by @charliermarsh on 2024-10-09 09:05_

That's great to hear :)

---
