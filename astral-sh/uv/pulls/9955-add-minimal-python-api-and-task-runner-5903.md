```yaml
number: 9955
title: "Add minimal Python API and task runner (#5903)"
type: pull_request
state: closed
author: chrisrodrigue
labels: []
assignees: []
draft: true
base: main
head: main
created_at: 2024-12-17T03:48:20Z
updated_at: 2024-12-19T10:29:55Z
url: https://github.com/astral-sh/uv/pull/9955
synced_at: 2026-01-12T16:09:03Z
```

# Add minimal Python API and task runner (#5903)

---

_@chrisrodrigue_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This introduces an ultra-minimal uv API and the concept of Tasks.

### uv API

The intent of the uv API is to provide the ability to use uv programmatically from Python. It consists of a single function `uv()` that shells out a string to the uv binary. The goal here is to generally support every conceivable uv command, option, and argument without a maintenance burden.

### Tasks API

What is a task? I think that it can be better understood as a user-defined command. The intent of the Tasks API is to allow a user to conveniently run their tasks in the context of their uv-managed project. It consists of a decorator `@task` to mark functions as tasks and a function `run_tasks` to autogenerate a CLI that can run their tasks.

#### `@task`

The `@task` decorator registers task names and functions in a dictionary. It optionally takes dependencies as a string using the first positional arg or `needs` kwarg. 

The decorator can be written multiple ways:
- `@task`
- `@task()`
- `@task("...")`
- `@task(needs="...")`

The optional `needs` argument can help to ensure that task dependencies are available in the uv-managed project before the task is run. It supports the same range of arguments and options as the `uv add` command. It can be used to add development dependencies, which most task dependencies probably should be, but the Tasks API does not make this assumption for the user.

I considered additional support for unpacking the args tuple and `Sequence` types (like `list` and `tuple`), but I felt that this provided too many ways to specify dependencies and those ways are visually noisier. I wanted using `@task()` and `uv()` to look and feel as natural as using the command line, and that can only be replicated with strings.

#### `run_tasks`

The `run_tasks` function inspects and uses properties of the decorated task functions to generate a CLI that can execute the tasks as subcommands.

Tasks are converted to CLI commands in the following fashion:
- Function docstrings are converted to command help/description text
- Function parameters without default values are converted to command arguments
- Function parameters with default values are converted to command options
- Function parameter type hints are converted to command option/argument type. **Note: It might be better to just default to strings here since there are a lot of edge cases to handle like union types and custom types.**

## Test Plan

<details>
  <summary>tasks.py</summary>

```python
from uv import run_tasks, task, uv


@task("ruff")
def check():
    """Run static analysis checks."""
    uv("run ruff check")


@task
def test():
    """Run unit tests."""
    uv("run pytest")


if __name__ == "__main__":
    run_tasks()
```

</details>

## Roadmap

### Task autodiscovery

On the roadmap could be the automatic discovery of user-created tasks to expose them as top-level uv commands. 

<details>
  <summary>Example task discovery</summary>

```bash
An extremely fast Python package manager.

Usage: uv [OPTIONS] <COMMAND>

Commands:
  run      Run a command or script
  init     Create a new project
  add      Add dependencies to the project
  remove   Remove dependencies from the project
  sync     Update the project's environment
  lock     Update the project's lockfile
  export   Export the project's lockfile to an alternate format
  tree     Display the project's dependency tree
  tool     Run and install commands provided by Python packages
  python   Manage Python versions and installations
  pip      Manage Python packages with a pip-compatible interface
  venv     Create a virtual environment
  build    Build Python packages into source distributions and wheels
  publish  Upload distributions to an index
  cache    Manage uv's cache
  self     Manage the uv executable
  version  Display uv's version
  help     Display documentation for a command

Tasks:
  check    Run static analysis checks [from: ./tasks.py]
  test     Run unit tests [from: ./tasks.py]
...
```
</details>

Currently the user is free to name and place their task module(s) however they'd like, but if autodiscovery is considered, a standardized name and place within the project structure might be beneficial for performance optimization. Configuration through environment variable or TOML is another option.

For security purposes, a user-defined task that shadows a uv command should:
- be forbidden
- print a warning
- not be executed by uv

## What about tasks in `pyproject.toml`?

Specifying tasks in `pyproject.toml` is not precluded with these changes,      but careful thought should be given to such a feature. Various properties might be desired to do things such as to stopping or continuing tasks on first failure, displaying task help text or descriptions, or composing multiple tasks.

## Availability 

These features are only available to users if `UV_PREVIEW` is set.

---

_Assigned to @zanieb by @zanieb on 2024-12-17 04:37_

---

_Comment by @chrisrodrigue on 2024-12-17 04:43_

Didn't see python tests in the codebase, were those something that you guys wanted to add at some point?

I wouldn't mind helping to set some up if you're open to it! I'm partial to pytest but can use whatever.

---

_Comment by @zanieb on 2024-12-17 15:32_

Thanks for exploring a concrete implementation! I'm going to move this to a draft, since we're not aligned on a design we aren't likely to be able to merge this in the short-term.

Regarding Python tests, they seem necessary for this approach — though I don't know if you should spend time on them yet. We don't have many now because we don't really have a Python API.

One concrete concern that comes to mind about this approach, if the tasks are defined in Python we're putting a pretty high floor on the overhead of executing tasks since we need to invoke a Python interpreter to discover them. This is a bit of a bummer, e.g., if we invoke `ruff` in a task defined in Python we'd significantly increase its runtime. This also makes things like "automatic discovery of user-created tasks to expose them as top-level uv commands" pretty infeasible.

---

_Converted to draft by @zanieb on 2024-12-17 15:32_

---

_Comment by @chrisrodrigue on 2024-12-17 22:42_

Thanks! I totally agree here.

For simple tasks and 80% of the use cases, defining tasks in TOML would probably be superior. Being able to read and cache tasks from a `pyproject.toml` section like `[tool.uv.tasks]` could provide the blazing performance that Astral is known for, even just to pass strings to `uv run`. `uv run` can call any other uv command and it can even call itself recursively without issues.

For the other 20% of use cases that require more complex tasks or workflows, I would still recommend Python. uv doesn’t manage the system packages or shell interpreter(s), but it does manage Python packages and interpreters. We know without a doubt that Python should be available on a system where uv is being used, but we can’t say the same for any other language or tool in which tasks can be defined. Inventing a new one seems like it would be way out of scope for uv. 

*POV: You are a uv-managed Python project that was just born with `uv init`. All you know about the outside world is that uv is your mommy and that you speak Python. The world beyond your directory tree is foreign, but you’re allowed to have as many friends as you want visit your virtual environment nest. Life in here is pretty sweet, and you think you’ll stay forever.*

---

_Closed by @chrisrodrigue on 2024-12-19 10:29_

---
