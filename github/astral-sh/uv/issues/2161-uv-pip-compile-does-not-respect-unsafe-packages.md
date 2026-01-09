---
number: 2161
title: "`uv pip compile` does not respect unsafe packages defined in `pyproject.toml`"
type: issue
state: closed
author: silv-io
labels:
  - configuration
assignees: []
created_at: 2024-03-04T16:05:49Z
updated_at: 2024-05-14T06:49:32Z
url: https://github.com/astral-sh/uv/issues/2161
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv pip compile` does not respect unsafe packages defined in `pyproject.toml`

---

_Issue opened by @silv-io on 2024-03-04 16:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
# Expected behavior

`pip-compile` has the feature to define "unsafe packages" which should not be pinned. This is useful when you want to make sure to always use the latest version of a specific package, regardless of the pin.

`uv` supports this feature, but only in the command line with the `--unsafe-package` option. However, `pip-tools` also scan the `pyproject.toml` section `tools.pip-tools` for the key `unsafe-package`.

It looks something like this:
```
[tool.pip-tools]
unsafe-package = ["some-package", "pip", "setuptools", "distribute"] # packages that should not be pinned
```
# Bug

`uv` does not read this section of the TOML and therefore produces different output on the same project. It should ideally read this section to offer drop-in replace-ability. Since this is in general a useful feature (not only for feature parity reasons), reading the equivalent key in "tools.uv", might be an option for the future as well. 

# Platform

* Linux AMD64
* `uv` version: `0.1.13`


---

_Comment by @zanieb on 2024-03-04 17:01_

Hi!

I don't think we're willing to read another tool's configuration. We're still formalizing a policy around this though.

We can also consider adding our own configuration section.

---

_Label `configuration` added by @zanieb on 2024-03-04 17:01_

---

_Comment by @silv-io on 2024-03-05 08:44_

Hi!

That's understandable. 

But as mentioned, it would be nice in general to have a way to set some parameters directly in the `pyproject.toml`. 
[Here's](https://github.com/jazzband/pip-tools?tab=readme-ov-file#configuration) the documentation section from `pip-tools` about that. 

Not having this is kind of blocking adoption for the tool because setting these manually on every call of `uv pip compile` can be error-prone. What this also allows for is auxiliary tooling, like a [pre-commit hook which checks if the lock file is still compatible with the project file](https://github.com/localstack/pre-commit-hooks). 
That said, does `uv` plan to have pre-commit hooks like that? Would be an awesome addition!

---

_Comment by @zanieb on 2024-03-05 15:11_

Yeah we'll have our own persistent configuration eventually, we're still determining what that will look like though.

And yeah a pre-commit hook is of interest to us.

---

_Comment by @silv-io on 2024-03-25 13:54_

@zanieb Do you have any update on when such a configuration will be available, or is there an official roadmap for that?
This is basically the only thing seriously blocking us from adopting `uv` now and we'd love to use it as it is so much faster :) 

---

_Comment by @silv-io on 2024-05-08 12:54_

@zanieb, @charliermarsh Any updates regarding persistent configuration? I see in https://github.com/astral-sh/uv/blob/main/docs/specifying_dependencies.md that you have some kind of configuration already in the `pyproject.toml`. I don't see however, whether there is support for setting something like this:

```
[tool.uv.pip.compile]
unsafe-package = ["some-package", "pip", "setuptools", "distribute"] # packages that should not be pinned
```

---

_Comment by @zanieb on 2024-05-08 13:37_

We are building persistent configuration right now, e.g. https://github.com/astral-sh/uv/pull/3007

---

_Comment by @silv-io on 2024-05-14 06:49_

@zanieb Looking through the toml schema, I found the possibility to define `no-emit-package` in the `pyproject.toml`.
This essentially does the same thing.

```
[tool.uv.pip]
no-emit-package = ["some-package", "pip", "setuptools", "distribute"] # packages that should not be pinned
```

I think this can be closed now! :) 

---

_Closed by @silv-io on 2024-05-14 06:49_

---
