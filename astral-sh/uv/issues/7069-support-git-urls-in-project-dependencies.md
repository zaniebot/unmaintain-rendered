```yaml
number: 7069
title: Support git URLs in project.dependencies
type: issue
state: closed
author: daviewales
labels:
  - question
assignees: []
created_at: 2024-09-05T06:28:11Z
updated_at: 2024-09-06T03:52:41Z
url: https://github.com/astral-sh/uv/issues/7069
synced_at: 2026-01-12T15:59:10Z
```

# Support git URLs in project.dependencies

---

_@daviewales_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

**EDIT**
Ignore the below. What I'm really asking for is a `uv` build backend. This will unlock the ability for private git packages to depend on other private git packages, and still be installed with `pip`, `pipx`, `poetry`, etc, without needing the package end user to change their tools.
**END EDIT**

Currently, if I try to add my private git repository as a dependency of another project, `uv` adds the repository to `[tool.uv.sources]`.
However, I can manually add the git dependency directly to `project.dependencies`, and it works OK:

``` toml
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "package_name@git+https://github.com/organisation/package-repository.git",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

This should enable packages which depend on this package, and which are installed by something other than `uv` to correctly fetch the required dependencies.

This syntax is supported by the underlying hatchling build system:
https://hatch.pypa.io/latest/config/dependency/#version-control-systems

Would it make more sense for `uv add package_name@git+https://github.com/organisation/package-repository.git` to just drop this package specifier directly into `project.dependencies`, rather than putting it somewhere which won't get picked up by other tools trying to install the current package?


```
❯ uv --version
uv 0.4.5 (42b6bfbad 2024-09-04)
```

---

_Comment by @daviewales on 2024-09-05 06:36_

This is also relevant to:

    uv add --script example.py package_name@git+https://github.com/organisation/package-repository.git

I'd expect to see:

```
# dependencies = [
#     package_name@git+https://github.com/organisation/package-repository.git
# ]
```

But instead I see:

```
# dependencies = [
#     package_name
# ]
#
# [tool.uv.sources]
# package_name = { git = "https://github.com/organisation/package-repository.git" }



---

_Comment by @charliermarsh on 2024-09-05 13:16_

You can pass `--raw-sources`, like `uv add --raw-sources`, to avoid writing to `tool.uv.sources`.

---

_Label `question` added by @charliermarsh on 2024-09-05 13:16_

---

_Comment by @daviewales on 2024-09-05 13:25_

Thanks, that's good to know. It's still kind of confusing that by default it puts it somewhere that's not going to be picked up by the hatchling build system when installed as a package by other package managers, even though it works perfectly well.

**Edit**: I realise I'm assuming that the hatchling build tool doesn't know to check `tool.uv.sources`. But this may be a wrong assumption?

Poetry uses its own dependencies section, rather than the standard dependencies section too.
However, poetry also sets itself up as the build tool, so when someone else tries to install my package which depends on a git package, they will automatically run the poetry build tool, which will know to look in the poetry dependencies section.

uv uses hatchling as the build tool. Does hatchling know to install git dependencies from `tool.uv.sources`?
Or will a package which specifies git dependencies in `tool.uv.sources` not be installable with something like `pip install`?

---

_Comment by @charliermarsh on 2024-09-05 13:56_

The thinking is that if you're using uv, you likely want to use the first-class uv features -- and also, that you _already_ can't publish such packages to PyPI, so there's less benefit in encoding the standardized metadata. (Whereas, e.g., the `tool.uv.sources` format is more powerful since we can encode the _type_ of request (tag, branch, etc.), which is more robust and more performant.)

But it's totally fine to use `--raw-sources`, there are no issues, it's just not the default.

> uv uses hatchling as the build tool. Does hatchling know to install git dependencies from tool.uv.sources?
> Or will a package which specifies git dependencies in tool.uv.sources not be installable with something like pip install?

It's a little nuanced but hatchling is a build backend, but what you're describing would arguably be the responsibility of a build frontend (the thing that prepares the environment and calls the build hooks). I guess it's somewhat debatable who is "responsible" for this part actually. But regardless, `pip install` would not properly pick up anything in `tool.uv.sources` (`uv pip install` would).


---

_Comment by @daviewales on 2024-09-06 00:17_

Thanks, on reflection, I think your last paragraph highlights my real concern.

Suppose I create a private package in GitHub which depends on another private package in GitHub.
(This is a real thing that I do for work!)

With poetry, I can add the project_one as a dependency of project_two as follows:

```
poetry add package_one@git+https://github.com/organisation/package-one-repository.git
```

This results in a `pyproject.toml` which looks like this:

``` toml
...
[tool.poetry.dependencies]
package_one = {git = "https://github.com/organisation/package-one-repository.git"}

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

On the surface, this appears to be the same thing that `uv` does.
The dependencies are specified in the non-standard `tool.poetry.dependencies` section, similar to how `uv` specifies git dependencies in  `[tool.uv.sources]`.

_However_, the critical difference is that the `poetry-core` build backend _knows_ about `tool.poetry.dependencies`.
So, when I run:

    pip install package_two@git+https://github.com/organisation/package-two-repository.git

`pip` reads the `pyproject.toml`, fetches `poetry-core` build backend, and then `poetry-core` automatically installs my private git dependency on `project_one`.

So, I can use `poetry` to build the package, and `poetry` can use whatever means to specify the dependencies it likes, but any other tool (`pip`, `pipx`, `uv`, etc) can transparantly install and use the package, including the private git dependencies.

My understanding is that if I built project_one and project_two with uv, and used the preferred `tool.uv.sources` syntax for git dependencies, that everything would work if someone uses `uv` to install `prjoect_two`, but it would not be possible to run:

    pip install package_two@git+https://github.com/organisation/package-two-repository.git

as the hatchling build backend doesn't know about `tool.uv.sources`, so it won't work. (Am I understanding that correctly?)

Thinking about this, I retract my suggestion to include the git URLs in `project.dependencies`.
I suspect that the real solution would be to provide a uv-aware build-backend which knows how to handle `tool.uv.sources`.
(If hatchling can already do this, then I'm sorry for the noise, and the issue can be closed!)


---

_Comment by @zanieb on 2024-09-06 00:43_

Yes what you've written there sounds correct. We have plans to build a backend, just uncertain on the timeline, ref #3957 

---

_Comment by @daviewales on 2024-09-06 03:52_

I've summarised this discussion on #3957, so I'll close this issue.

---

_Closed by @daviewales on 2024-09-06 03:52_

---
