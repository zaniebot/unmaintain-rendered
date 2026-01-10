```yaml
number: 8690
title: "`INP001`: Skip scripts, for example Python files with shebang"
type: issue
state: closed
author: sanmai-NL
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-11-15T09:35:44Z
updated_at: 2023-11-16T22:21:35Z
url: https://github.com/astral-sh/ruff/issues/8690
synced_at: 2026-01-10T11:09:50Z
```

# `INP001`: Skip scripts, for example Python files with shebang

---

_Issue opened by @sanmai-NL on 2023-11-15 09:35_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

A Python _script_ isn't meant to be imported as a package, and so INP001 is not applicable.

![image](https://github.com/astral-sh/ruff/assets/3374183/d6289230-e5b8-4b5d-8994-55191dafd36c)

There are multiple criteria that each imply a file is a script, in particular in combination:

- It contains a shebang (not necessary but sufficient).
- It's outside the `src` directory (only necessary and sufficient if you stick to the [src-layout](https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#src-layout), perhaps).

ruff 0.1.5.

---

_Comment by @sanmai-NL on 2023-11-15 10:00_

Work-around for `src/` criterion in `pyproject.toml`. This ignores `INP001` for all files not under `src/`.

```toml
[tool.ruff.per-file-ignores]
"[!s][!r][!c]*/**" = ["INP001"]
```

---

_Comment by @charliermarsh on 2023-11-15 16:38_

I thought (2) was already respected to some degree -- we skip files that are direct children of the project root, and of any of the declared `src` directories. This tends to catch most scripts. I'm wondering if declaring your `.gitlab-ci` directory as a source root would solve the problem here?

---

_Comment by @sanmai-NL on 2023-11-15 16:51_

Do you mean skipped when traversing a root target directory? A CI tool/Git hook may specify a Python file path explicitly. 

---

_Comment by @charliermarsh on 2023-11-15 16:53_

I mean if you have a source set, like:
```toml
[tool.ruff]
src = ["src", ".gitlab-ci"]
```
Then any files that are direct children of those directories get skipped.

---

_Comment by @charliermarsh on 2023-11-16 00:51_

We could ignore files with shebangs though.

---

_Label `rule` added by @charliermarsh on 2023-11-16 00:51_

---

_Label `needs-decision` added by @charliermarsh on 2023-11-16 00:51_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-16 01:12_

---

_Closed by @charliermarsh on 2023-11-16 22:21_

---
