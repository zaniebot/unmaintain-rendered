---
number: 9071
title: Docs lack info on setting up local tools
type: issue
state: open
author: jbullion-astra
labels:
  - documentation
  - question
assignees: []
created_at: 2024-11-12T21:23:02Z
updated_at: 2024-12-14T20:46:01Z
url: https://github.com/astral-sh/uv/issues/9071
synced_at: 2026-01-10T01:24:36Z
---

# Docs lack info on setting up local tools

---

_Issue opened by @jbullion-astra on 2024-11-12 21:23_

The docs have 3 pages that document "tools".

- https://docs.astral.sh/uv/concepts/tools/ 
- https://docs.astral.sh/uv/guides/tools/ 
- https://docs.astral.sh/uv/reference/cli/#uv-tool 

These docs don't proved examples of how to set up a project (or scripts in a project) as a tool. 

Documentation on how tool discovery would also be appreciated. 

My current pyproject.toml:
```toml
[project]
name = "mytool"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "rich>=13.9.4",
]

[project.scripts]
mytool = "mytool:main"
```

How do I run and install a local tool? 

---

_Comment by @zanieb on 2024-11-13 01:30_

I believe you're looking for `uv tool install -e .`. You'll need to configure a [build system](https://docs.astral.sh/uv/concepts/projects/#build-systems) too.

---

_Label `documentation` added by @zanieb on 2024-11-13 01:31_

---

_Label `question` added by @zanieb on 2024-11-13 01:31_

---

_Comment by @pchalasani on 2024-12-14 18:31_

Agree with the original post -- I'm at a loss on how to create a tool `foo` for my open-source library `blah` so that people can directly run `foo` using `uvx blah foo` or `uvx --from blah foo` from any directory without first having to install `blah`. 


---

_Comment by @zanieb on 2024-12-14 20:16_

@pchalasani that's just defining a CLI for your library https://docs.astral.sh/uv/concepts/projects/config/#command-line-interfaces — is your library published?

---

_Comment by @pchalasani on 2024-12-14 20:46_

Thanks @zanieb I somehow didn't find this page in the docs, but found the same instructions elsewhere. 
Yes my library `langroid` is published, and I have a separate examples repo where I wanted to set up the cli examples to be easily runnable via `uvx` :
https://github.com/langroid/langroid-examples?tab=readme-ov-file#run-scripts-with-no-repo-cloning-no-venv-setup

There were some hiccups initially since I didn't realize that although my examples are runnable as a module, e.g. `python3 -m examples.basic.chat`, when I set up the entry points for cli, I need to add `:app` since my examples use `typer.app`.

It may help to point to this CLI page in your docs where you talk about "tools" (which seem the same thing or similar, which actually I'm not sure)


---
