```yaml
number: 9011
title: "Question: What’s the difference between `optional-dependencies` and `dependency-groups` in `pyproject.toml`?"
type: issue
state: closed
author: pplmx
labels:
  - question
assignees: []
created_at: 2024-11-11T09:54:33Z
updated_at: 2025-06-17T15:40:09Z
url: https://github.com/astral-sh/uv/issues/9011
synced_at: 2026-01-10T03:32:44Z
```

# Question: What’s the difference between `optional-dependencies` and `dependency-groups` in `pyproject.toml`?

---

_Issue opened by @pplmx on 2024-11-11 09:54_

Hi,

I have a question regarding the configuration of dependencies in `pyproject.toml`. 

I’ve noticed that both `optional-dependencies` and `dependency-groups` seem to handle optional dependencies in a similar way, so I'm a bit confused about how they differ and when each should be used. Could you clarify the following points?

- **Purpose**: What is the intended purpose of `optional-dependencies` versus `dependency-groups`?

- **Usage Scenarios**: In what specific scenarios would it make sense to use one over the other?

Any insights or examples you could provide would be really helpful.

Thanks in advance for your time and assistance!


---

_Comment by @PRFina on 2024-11-11 11:33_

I was wondering about the same today, looking for some insights here. From my understanding:

* `optional-dependencies` (aka package's extras) are meant to be shipped with your code. However, installing them is optional as they provide "extra" features of your package, maybe supporting a smaller set of users. Think about how pandas handle them. Most users want to work with their core API, ie, with `Dataframe` or `Series` data structures with some locally stored csv/parquet files. However, some user wants to have extra features to read/write the data from/to some remote storage. To fulfill this purpose they provide `pip install pandas[aws,gcp,fss]` 

* `dependencies-groups` are still optional dependencies, but are not meant to be shipped with your package. I'm thinking about all those dependencies that help to support your package development activities (testing, linting, formatting, QA, etc,). Indeed they're called dev dependencies [in the doc](https://docs.astral.sh/uv/concepts/dependencies/#development-dependencies). For example, I know that the data scientist who works with me like to prototype some code in Jupyter Notebooks first, and then move their code snippets in some py module. Therefore, I'm adding Jupyter as a `dependencies-groups` dependency since it's not part of the package itself, just the dev workflow.
 
I think it's more about what ends up in the published package than how they are handled by uv. In theory, you can move all your `optional-dependencies` into the `optional-dependencies` table and vice-versa, ending up with the same amount of resolved/installed packages in your venv.

Moreover `optional-dependencies` are part of the [the pep 621 standard specification](https://packaging.python.org/en/latest/specifications/pyproject-toml/#declaring-project-metadata-the-project-table) while `dependencies-groups` is uv specific. Therefore, the former also works with other pep 621 compliant tools (eg. pip) while the latter only with uv. 

I'm new to uv, so they are just considerations. Probably some contributor could shed more light on the rationale behind this feature :smiley: 

---

_Comment by @Ravencentric on 2024-11-11 12:00_

> Moreover optional-dependencies are part of the the pep 621 standard specification while dependencies-groups is uv specific. Therefore, the former also works with other pep 621 compliant tools (eg. pip) while the latter only with uv.

They are not uv specific but a fairly new packaging standard defined by [PEP 735 – Dependency Groups in pyproject.toml](https://peps.python.org/pep-0735/).

`project.optional-dependencies` are part of your published package and can be installed by the end user via `pip install your-package[some-extra]` while dependency groups are not published with your package.

---

_Comment by @PRFina on 2024-11-11 13:37_

Thanks for pointing it out @Ravencentric, I was not aware of it :smile:

---

_Comment by @pplmx on 2024-11-11 13:43_

@PRFina Thanks for your response; it was really helpful!

---

_Label `question` added by @charliermarsh on 2024-11-11 13:48_

---

_Comment by @charliermarsh on 2024-11-11 13:48_

See also: https://github.com/astral-sh/uv/issues/8981#issuecomment-2466787211.

---

_Closed by @charliermarsh on 2024-11-11 13:48_

---

_Comment by @majidaldo on 2025-01-02 16:36_

Wouldn't this violate DRY? If I'm working on an optional user feature, how do I specify the deps as a `dependency-group` as well as a `project.optional-dependencies ` without repeating?

---

_Comment by @zanieb on 2025-01-02 17:39_

If it's a user feature, use `project.optional-dependencies` instead of `dependency-group`.

---

_Comment by @zanieb on 2025-01-02 17:43_

uv does support this though

```
❯ uv init example
Initialized project `example` at `/Users/zb/workspace/uv/example`

❯ cd example
❯ uv add --optional feat anyio
Using CPython 3.12.8 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtual environment at: .venv
Resolved 5 packages in 114ms
Prepared 4 packages in 97ms
Installed 4 packages in 2ms
 + anyio==4.7.0
 + idna==3.10
 + sniffio==1.3.1
 + typing-extensions==4.12.2

❯ uv add --group feat 'example[feat]'
Resolved 5 packages in 2ms
Audited 4 packages in 0.00ms

❯ cat pyproject.toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[project.optional-dependencies]
feat = [
    "anyio>=4.7.0",
]

[dependency-groups]
feat = [
    "example[feat]",
]

❯ uv sync --group feat
Resolved 5 packages in 1ms
Installed 4 packages in 6ms
 + anyio==4.7.0
 + idna==3.10
 + sniffio==1.3.1
 + typing-extensions==4.12.2
```

---

_Comment by @majidaldo on 2025-01-02 17:54_

> uv does support this though
> 
> ```
> ❯ uv init example
> Initialized project `example` at `/Users/zb/workspace/uv/example`
> 
> ❯ cd example
> ❯ uv add --optional feat anyio
> Using CPython 3.12.8 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
> Creating virtual environment at: .venv
> Resolved 5 packages in 114ms
> Prepared 4 packages in 97ms
> Installed 4 packages in 2ms
>  + anyio==4.7.0
>  + idna==3.10
>  + sniffio==1.3.1
>  + typing-extensions==4.12.2
> 
> ❯ uv add --group feat 'example[feat]'
> Resolved 5 packages in 2ms
> Audited 4 packages in 0.00ms
> 
> ❯ cat pyproject.toml
> [project]
> name = "example"
> version = "0.1.0"
> description = "Add your description here"
> readme = "README.md"
> requires-python = ">=3.12"
> dependencies = []
> 
> [project.optional-dependencies]
> feat = [
>     "anyio>=4.7.0",
> ]
> 
> [dependency-groups]
> feat = [
>     "example[feat]",
> ]
> 
> ❯ uv sync --group feat
> Resolved 5 packages in 1ms
> Installed 4 packages in 6ms
>  + anyio==4.7.0
>  + idna==3.10
>  + sniffio==1.3.1
>  + typing-extensions==4.12.2
> ```

oh ok! b/c i can refer to the package that i'm developing. thanks!

---

_Comment by @zanieb on 2025-01-02 18:03_

I'm not sure if other tools will accept that.

---

_Comment by @lucharo on 2025-03-31 09:52_

can i use dependency groups with `uv pip`? if so what's the pip compatible syntax? 

My use case is dev dependencies so based on what I've read in this thread dependency group seem better suited but I often will install my package and all of it's dependencies via editable installs (`uv pip install -e .[dev]` - in the optional dependency syntax - my understand is that editable installs are not yet support in `uv`'s native API, is it?)

---

_Comment by @zanieb on 2025-04-01 21:34_

@lucharo you can install groups with `uv pip install --group dev`. In the native API, we use editable installs by default for the project (and any workspace members) in `uv sync`.



---

_Comment by @lucharo on 2025-04-02 06:17_

Ah that's great!! Thanks @zanieb 

I'll move away from `pip install -e ".[dev]"` then 

And you're saying that `uv sync` does editable installs? 

---

_Comment by @zanieb on 2025-04-02 14:16_

Yeah `uv sync` does editable installs.

---

_Comment by @lucharo on 2025-04-03 06:04_

> Yeah `uv sync` does editable installs.

Fantastic! How do you handle optional and group dependencies with `uv sync`? 

If `uv sync` is equivalent to `pip install -e .` 

What's the command to do `pip install -e .[extra]` or `pip install -e . --group dev`?

---

_Comment by @lucharo on 2025-04-03 07:39_

my bad, it's in the help:

I use `uv sync --all-extras`

```sh
$ uv sync --help   
Update the project's environment

Usage: uv sync [OPTIONS]

Options:
      --extra <EXTRA>                            Include optional dependencies from the specified extra name
      --all-extras                               Include all optional dependencies
      --no-extra <NO_EXTRA>                      Exclude the specified optional dependencies, if `--all-extras` is supplied
      --no-dev                                   Disable the development dependency group
      --only-dev                                 Only include the development dependency group
      --group <GROUP>                            Include dependencies from the specified dependency group
      --no-group <NO_GROUP>                      Disable the specified dependency group
      --no-default-groups                        Ignore the default dependency groups
      --only-group <ONLY_GROUP>                  Only include dependencies from the specified dependency group
      --all-groups                               Include dependencies from all dependency groups
      --no-editable                              Install any editable dependencies, including the project and any workspace
                                                 members, as non-editable
      --inexact                                  Do not remove extraneous packages present in the environment
      --active                                   Sync dependencies to the active virtual environment
      --no-install-project                       Do not install the current project
      --no-install-workspace                     Do not install any workspace members, including the root project
      --no-install-package <NO_INSTALL_PACKAGE>  Do not install the given package(s)
      --locked                                   Assert that the `uv.lock` will remain unchanged [env: UV_LOCKED=]
      --frozen                                   Sync without updating the `uv.lock` file [env: UV_FROZEN=]
      --dry-run                                  Perform a dry run, without writing the lockfile or modifying the project
                                                 environment
      --all-packages                             Sync all packages in the workspace
      --package <PACKAGE>                        Sync for a specific package in the workspace
      --script <SCRIPT>                          Sync the environment for a Python script, rather than the current project
      --check                                    Check if the Python environment is synchronized with the project
```

---

_Comment by @Alex-ley-scrub on 2025-05-02 00:09_

you can also use this (which allows install into current active conda env for example and doesn't force venv to be created):
```
uv pip install -e . --all-extras -r pyproject.toml
```
these all seems to force the creation of a venv, which I personally don't want:
```
uv sync --all-extras --active
uv sync --all-extras --active --system # system no longer an option? https://docs.astral.sh/uv/pip/environments/#using-arbitrary-python-environments
uv sync --all-extras --active --python $(which python)
```

I'd love to be wrong and/or to learn how I could use `uv sync` without it creating a new venv and just installing into the current active env:

<img width="594" alt="Image" src="https://github.com/user-attachments/assets/18e1055e-60dc-4a29-a35c-2748fa353505" />

<img width="458" alt="Image" src="https://github.com/user-attachments/assets/bebd0f93-7bf4-4f57-acfd-9a8895084488" />

<img width="577" alt="Image" src="https://github.com/user-attachments/assets/9c89d571-38af-460f-b0d3-09bd492bb4c4" />


---

_Comment by @pypae on 2025-06-17 15:40_

> uv does support this though
> 
> ```
> ❯ uv init example
> Initialized project `example` at `/Users/zb/workspace/uv/example`
> 
> ❯ cd example
> ❯ uv add --optional feat anyio
> Using CPython 3.12.8 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
> Creating virtual environment at: .venv
> Resolved 5 packages in 114ms
> Prepared 4 packages in 97ms
> Installed 4 packages in 2ms
>  + anyio==4.7.0
>  + idna==3.10
>  + sniffio==1.3.1
>  + typing-extensions==4.12.2
> 
> ❯ uv add --group feat 'example[feat]'
> Resolved 5 packages in 2ms
> Audited 4 packages in 0.00ms
> 
> ❯ cat pyproject.toml
> [project]
> name = "example"
> version = "0.1.0"
> description = "Add your description here"
> readme = "README.md"
> requires-python = ">=3.12"
> dependencies = []
> 
> [project.optional-dependencies]
> feat = [
>     "anyio>=4.7.0",
> ]
> 
> [dependency-groups]
> feat = [
>     "example[feat]",
> ]
> 
> ❯ uv sync --group feat
> Resolved 5 packages in 1ms
> Installed 4 packages in 6ms
>  + anyio==4.7.0
>  + idna==3.10
>  + sniffio==1.3.1
>  + typing-extensions==4.12.2
> ```

That's exactly what I was looking for, but took me quite a while to figure out. 

I combined the above with

```
[tool.uv]
default-groups = "all"
```

So I don't have to type `uv sync --all-extras` during development.

Maybe adding a small note to https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-groups or https://docs.astral.sh/uv/concepts/projects/dependencies/#optional-dependencies would help?

---
