```yaml
number: 13027
title: "`ruff check --show-files` does not consider `lint.exclude` config"
type: issue
state: open
author: Enayaaa
labels:
  - cli
  - needs-design
assignees: []
created_at: 2024-08-21T09:19:13Z
updated_at: 2024-08-23T05:48:33Z
url: https://github.com/astral-sh/ruff/issues/13027
synced_at: 2026-01-12T15:54:52Z
```

# `ruff check --show-files` does not consider `lint.exclude` config

---

_@Enayaaa_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
`ruff check --show-files` has become a confusing option because it is a flag for the `check` command. I first assumed it did consider the `lint.exclude` config but it doesn't. I guess this only made sense when `ruff check` was just `ruff`.

```shell
user ~ ➜  ruff --version
ruff 0.6.1
```
pyproject.toml
```toml
[tool.ruff]
exclude = []
[tool.ruff.format]
exclude = []
[tool.ruff.lint]
exclude = ["src"]
```
current behaviour
```shell
user ~/Documents/my-repo (master) ➜  ruff check --show-files
/home/user/Documents/my-repo/docs/source/__init__.py
/home/user/Documents/my-repo/docs/source/conf.py
/home/user/Documents/my-repo/noxfile.py
/home/user/Documents/my-repo/pyproject.toml
/home/user/Documents/my-repo/setup.py
/home/user/Documents/my-repo/src/__init__.py
/home/user/Documents/my-repo/src/_version.py
/home/user/Documents/my-repo/src/config/__init__.py
/home/user/Documents/my-repo/src/release_hooks.py
/home/user/Documents/my-repo/src/src.py
/home/user/Documents/my-repo/src/utils/__init__.py
/home/user/Documents/my-repo/src/utils/nox.py
/home/user/Documents/my-repo/src/utils/packaging.py
/home/user/Documents/my-repo/src/utils/path.py
/home/user/Documents/my-repo/tests/__init__.py
/home/user/Documents/my-repo/tests/test_nox_utils.py
/home/user/Documents/my-repo/tests/test_packaging.py
/home/user/Documents/my-repo/tests/test_path.py
```
Expected behaviour: Files under `my-repo/src` not to be in this list.

If we consider changing this behaviour to consider the `lint.exclude` rule and similiar flags, then we should add a separate and equivalent flag for `ruff format` that considers the `format.exclude` config and similiar flags #13006.

---

_Label `cli` added by @MichaReiser on 2024-08-21 09:23_

---

_Label `needs-design` added by @MichaReiser on 2024-08-21 09:23_

---

_Comment by @dhruvmanila on 2024-08-23 05:48_

I think this is a bug because we do consider the `exclude` / `extend-exclude` option in the top level section (`[tool.ruff]`) and might be related to https://github.com/astral-sh/ruff/issues/8267.

---
