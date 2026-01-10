```yaml
number: 6795
title: "Wrong `python_full_version` in `uv.lock`"
type: issue
state: closed
author: TheRealBecks
labels:
  - question
assignees: []
created_at: 2024-08-29T07:15:37Z
updated_at: 2024-08-29T17:00:24Z
url: https://github.com/astral-sh/uv/issues/6795
synced_at: 2026-01-10T04:45:09Z
```

# Wrong `python_full_version` in `uv.lock`

---

_Issue opened by @TheRealBecks on 2024-08-29 07:15_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I have the following `pyproject.toml`:
```
[project]
name = "lightning"
description = "Ansible Network Automation for Cisco IOS, Cisco NX-OS and Arista EOS"
readme = "README.md"
version = "1.0.0"
requires-python = "==3.12"
dependencies = [
  "ansible",
  "ansible-core",
  "ansible-navigator",
  "ansible-pylibssh",
  "hvac",
  "jmespath",
  "netaddr",
  "paramiko",
  "pyavd[ansible]",
]

[project.optional-dependencies]
dev = [
  "icecream",
  "ruff==0.6.2",
]
```
`requires-python` is hard coded to version `==3.12`.

With `uv sync` (without any extras to be installed) a `uv.lock` has been created with the following data in the first few lines:
```
version = 1
requires-python = "==3.12"
resolution-markers = [
    "python_full_version < '3.13'",
    "python_full_version >= '3.13'",
]
```
The second line of `python_full_version` looks wrong as it declares the version to be `>= '3.13'` while the first one says `< '3.13'`.

I used `uv` in version `0.3.5` on Opensuse Tumbleweed Linux.



---

_Comment by @TheRealBecks on 2024-08-29 07:28_

I also tried version `0.4.0`, but that does not change the `python_full_version`.

---

_Comment by @charliermarsh on 2024-08-29 12:53_

`resolution-markers` is effectively internal bookkeeping for the resolver. Are you seeing an incorrect resolution?

---

_Label `question` added by @charliermarsh on 2024-08-29 12:53_

---

_Comment by @zanieb on 2024-08-29 13:56_

Generally it's expected for these markers to be disjoint. See the documentation on forking at https://docs.astral.sh/uv/reference/resolver-internals/#forking

---

_Comment by @TheRealBecks on 2024-08-29 14:08_

I requested Python 3.12 (12!), but the `resolution-markers` are:

1. `< '3.13'` --> That's correct for version 3.12
2. `>= '3.13'` --> That's **wrong** for version 3.12. I expected `>= '3.12'`

So **I think** that's wrong? 😀

---

_Comment by @charliermarsh on 2024-08-29 14:09_

That's still ok! It's internal state that represents how we resolved the dependencies. It doesn't effect the outcome when you run `uv sync` or similar.

---

_Comment by @TheRealBecks on 2024-08-29 14:11_

Okay, if you say so then this issue/question can be closed! 😀

Thanks a lot for the fast response!

---

_Closed by @TheRealBecks on 2024-08-29 14:12_

---

_Comment by @zanieb on 2024-08-29 17:00_

Related #6150 

---
