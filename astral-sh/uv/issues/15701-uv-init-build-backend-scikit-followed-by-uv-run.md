```yaml
number: 15701
title: uv init --build-backend scikit followed by uv run --with jupyter jupyter lab has counterintuitive behaviour concerning rebuilds
type: issue
state: closed
author: rimathia
labels:
  - question
assignees: []
created_at: 2025-09-05T11:08:57Z
updated_at: 2025-09-05T15:21:47Z
url: https://github.com/astral-sh/uv/issues/15701
synced_at: 2026-01-10T03:23:54Z
```

# uv init --build-backend scikit followed by uv run --with jupyter jupyter lab has counterintuitive behaviour concerning rebuilds

---

_Issue opened by @rimathia on 2025-09-05 11:08_

### Question

This is probably just a documentation request. My first step is
`uv init --build-backend scikit`
which works as expected, next I'm starting a jupyter notebook using
`uv run --with jupyter jupyter lab`
which also works as expected, it builds the C++ extension and I can now create a notebook (using the default kernel) where I can load the C++ python extension. This is a massive improvement over any other tool or workflow that I've tried.
However, I haven't been able to find out the right way to invoke uv to create a jupyter kernel such that it rebuilds the C++ extension if the source files have changed. One way which does work is
`uv clean && rm -r .venv && uv run --with jupyter jupyter lab`
but that is certainly not the way to do it. 

Maybe the uv init --build-backend scikit could document the right invocations (for example in the README.md which it creates which is empty by default) or it could print the two or three most frequent workflow commands to the command line after it has created the environment.

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.7.19 (38ee6ec80 2025-07-02)

---

_Label `question` added by @rimathia on 2025-09-05 11:08_

---

_Comment by @konstin on 2025-09-05 12:24_

Are you looking for the `--refresh-package` option?

---

_Comment by @rimathia on 2025-09-05 14:47_

Thanks, `--refresh-package` doesn't recompile the C++ code, but `--reinstall` does. Given how fast the package reinstallation is, that is perfecly fine for a edit-compile-test cycle with a jupyter notebook.
I'll close this. There are certainly good reasons why a change to the `pyproject.toml` triggers changes to the python environment for other commands than changes to the python extension source.

---

_Closed by @rimathia on 2025-09-05 14:47_

---

_Comment by @konstin on 2025-09-05 15:12_

The reason is mostly that we know that `pyproject.toml` files should rebuild, but we don't know which other files are relevant. You can control this behavior with https://docs.astral.sh/uv/reference/settings/#cache-keys.

---

_Comment by @rimathia on 2025-09-05 15:21_

Thanks for the bonus tip, using
`[tool.uv]
cache-keys = [{ file = "pyproject.toml" }, { file = "requirements.txt" },  { file = "src/**.*" }]`
I get exactly the behaviour that I intuitively expected: every change of a source file triggers a rebuild of the python extension.

---
