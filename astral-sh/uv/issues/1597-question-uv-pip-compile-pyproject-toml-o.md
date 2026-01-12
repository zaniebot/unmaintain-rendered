```yaml
number: 1597
title: "Question: `uv pip compile pyproject.toml -o requirements.txt` generate empty `requirements.txt`"
type: issue
state: closed
author: XiaoConstantine
labels:
  - question
  - compatibility
assignees: []
created_at: 2024-02-17T15:43:43Z
updated_at: 2024-03-25T20:30:42Z
url: https://github.com/astral-sh/uv/issues/1597
synced_at: 2026-01-12T15:58:30Z
```

# Question: `uv pip compile pyproject.toml -o requirements.txt` generate empty `requirements.txt`

---

_@XiaoConstantine_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Context:
--------
Trying to convert a simple poetry project with uv.

```bash
❯ uname -a
Darwin Xiaos-MBP.fios-router.home 23.2.0 Darwin Kernel Version 23.2.0: Wed Nov 15 21:53:34 PST 2023; root:xnu-10002.61.3~2/RELEASE_ARM64_T8103 arm64
```

```bash
❯ uv -V
uv 0.1.3
```

My `pyproject.toml` file:
```toml
[tool.poetry.dependencies]
python = "^3.9"
torch = "^1.13.1"
huggingface = "^0.0.1"
transformers = {extras = ["torch"], version = "^4.37.1"}
```

Running following commands:
1. Create venv:
```bash
uv venv
```
2. Activate venv:
```bash
source ./venv/bin/activate
```
3. Compile the requriements txt from pyproject toml
```❯ uv -v pip compile  pyproject.toml -o requirements.txt
 uv::requirements::from_source source=pyproject.toml
 uv::requirements::from_source source=requirements.txt
warning: Requirements file requirements.txt does not contain any dependencies
    0.007549s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTUAL_ENV at: /Users/xiao/development/github.com/XiaoConstantine/butler/.venv
    0.008561s DEBUG uv_interpreter::interpreter Using cached markers for: /Users/xiao/development/github.com/XiaoConstantine/butler/.venv/bin/python
    0.008567s DEBUG uv::commands::pip_compile Using Python 3.9.16 interpreter at /Users/xiao/development/github.com/XiaoConstantine/butler/.venv/bin/python for builds
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.163449s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.9.16
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
Resolved 0 packages in 164ms
```
Also tried with `uv -n` (no-cache) but seems the same result

Is this due to the dependency or I missed something? Thanks in advance!



---

_Comment by @zanieb on 2024-02-17 16:30_

Hi! Thanks for giving the project a try. We don't read Poetry's dependency format, just the [PEP 621](https://peps.python.org/pep-0621/#dependencies-optional-dependencies) standard.

---

_Label `question` added by @zanieb on 2024-02-17 16:31_

---

_Label `compatibility` added by @zanieb on 2024-02-17 16:31_

---

_Comment by @XiaoConstantine on 2024-02-17 17:05_

Ah makes sense! Thank you!

---

_Closed by @XiaoConstantine on 2024-02-17 17:05_

---

_Comment by @zanieb on 2024-03-25 20:30_

As of https://github.com/astral-sh/uv/pull/2633 we support reading this directly.

---
