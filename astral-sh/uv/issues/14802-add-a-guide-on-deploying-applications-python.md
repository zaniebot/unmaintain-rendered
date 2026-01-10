---
number: 14802
title: Add a guide on deploying applications Python projects
type: issue
state: open
author: ketozhang
labels:
  - question
assignees: []
created_at: 2025-07-22T01:24:59Z
updated_at: 2025-07-22T01:32:58Z
url: https://github.com/astral-sh/uv/issues/14802
synced_at: 2026-01-10T01:25:49Z
---

# Add a guide on deploying applications Python projects

---

_Issue opened by @ketozhang on 2025-07-22 01:24_

### Question

I want to ask if we can have a guide focused on deploying a project which is a Python applications and must be deployed in a reproducible manner. An application is any Python project that users are meant to interface it by calling an entry point rather than interfaced in code (as libraries would). Reproducible here limits to installing and running the app using a the same set of dependencies and Python version (if specified).

Currently, you can go quite far in deploying Python project as an application with these concept pages:

- How to define app dependencies
  - https://docs.astral.sh/uv/concepts/projects/dependencies/
- How to define and install reproducible app environments
  - https://docs.astral.sh/uv/concepts/projects/sync/
  - https://docs.astral.sh/uv/pip/compile
- How to start the application on the environment
  - https://docs.astral.sh/uv/concepts/projects/run/ 

There is walkthrough in the [Working on projects](https://docs.astral.sh/uv/guides/projects/) guide. However this guide focuses on package distribution and not application deployment. Something like this is often quite opinionated and not every deployment possibility can be covered

---

To demonstrate the difference, here's a snippet I'd imagine to be in the guide with sections that would differ from [Working on projects](https://docs.astral.sh/uv/guides/projects/). I intentionally chose to use `pylock.toml` over `uv.lock` mostly due to my familiarity (and a bit of wishful thinking).

<details><summary>Details</summary>


## Creating a New Project

```sh
$ uv init --app myapp
$ cd myapp
```

## Adding an application entry point

```toml
[project.scripts]
myapp = "myapp:main"
```

## Generating the application lock file

```sh
$ echo "." | uv pip compile -o pylock.toml -
```

> [!NOTE]
> The alternative is to generate the lock file without the project in its list
>
>   ```sh
>   $ uv pip compile -o pylock.toml pyproject.toml
>   ```
> During install, you will have to install both the lock and the project
> 
>   ```sh
>   $ uv pip install pylock.toml
>   $ uv pip install . --no-deps
>  ```

## Deploying the application
If we want to deploy our application say in another server, we need to first copy the application project into the server (e.g., using git). 

```sh
$ uv pip sync pylock.toml
$ uv run myapp start --port 443
```

## Distributing your application as a Python package

You may upload your application as a Python package to PyPI by [building your project](https://docs.astral.sh/uv/concepts/projects/build/).

However uv and Python package do not support including lock files in the package such that you can distribute reproducible installs.

Consider alternatives: [foo](#) and [bar](#)

</details> 

---

_I'd like to contribute but someone more familiar with the project is best to write this_


### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @ketozhang on 2025-07-22 01:25_

---
