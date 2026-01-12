```yaml
number: 7097
title: Specify URL dependencies with environment markers
type: issue
state: closed
author: tudoroancea
labels:
  - question
assignees: []
created_at: 2024-09-05T20:17:30Z
updated_at: 2024-09-06T08:46:19Z
url: https://github.com/astral-sh/uv/issues/7097
synced_at: 2026-01-12T15:59:10Z
```

# Specify URL dependencies with environment markers

---

_@tudoroancea_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Hi there, 

First let me thank you for all the incredible work you put into uv, it really is becoming a tool I am trying to use in every one of my python projects. One use case I still can't really solve with uv is the following: I have a dependency that does not have (yet) a published PyPI wheel (it's a nightly release) and only wheels available through a [GitHub release](https://github.com/casadi/casadi/releases/nightly-main). 
I wanted to know if there was a way to specify an URL dependency that can incorporate environment markers to provide one wheel URL for Linux and one for macOS. 
Based on the documentation it doesn't look like it, but it would definitely be useful for using dependencies to scientific packages (or any non-pure-python packages) in a truly cross-platform way.

Thanks in advance,

Ted

---

_Comment by @charliermarsh on 2024-09-05 20:38_

I think you should be able to do something like this:

```toml
dependencies = [
   "package_name @ https://github.com/path/to/wheel.whl ; sys_platform == 'darwin'",
   "package_name @ https://github.com/path/to/other_wheel.whl ; sys_platform == 'linux'",
]
```

---

_Label `question` added by @charliermarsh on 2024-09-05 20:39_

---

_Comment by @tudoroancea on 2024-09-06 08:46_

This works after also adding the following in the `pyproject.toml`:
```toml
[tool.hatch.metadata]
allow-direct-references = true # to allow the syntax 'pkg @ url' in dependencies
```
Thanks!

---

_Closed by @tudoroancea on 2024-09-06 08:46_

---
