```yaml
number: 12058
title: "Relative paths in `per-file-ignores` not respected when running `ruff` outside working directory using `--config` with `pyproject.toml`"
type: issue
state: open
author: mihirsamdarshi
labels:
  - configuration
assignees: []
created_at: 2024-06-27T00:19:37Z
updated_at: 2024-07-13T07:12:05Z
url: https://github.com/astral-sh/ruff/issues/12058
synced_at: 2026-01-10T11:09:54Z
```

# Relative paths in `per-file-ignores` not respected when running `ruff` outside working directory using `--config` with `pyproject.toml`

---

_Issue opened by @mihirsamdarshi on 2024-06-27 00:19_

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

## Description

[I noticed this (potential) issue](https://github.com/koxudaxi/ruff-pycharm-plugin/issues/359#issuecomment-2192785432) while debugging koxudaxi/ruff-pycharm-plugin#359 for PyCharm. It's may be expected behavior, so please close this issue if so.

When running `ruff` from a directory that is not the same as the one containing the `pyproject.toml` file and specifying the `--config` option, the relative paths specified in the `pyproject.toml`'s `per-file-ignores` configuration are not respected.

This normally isn't a problem, since the `pyproject.toml` lives in the root of project files, but I have a full-stack application where the python source code does not live in the source code [(see the issue above)](https://github.com/koxudaxi/ruff-pycharm-plugin/issues/359#issuecomment-2192785432). I open this root folder in PyCharm. Unfortunately, the `ruff-pycharm-plugin` runs `ruff` from the project's root directory, and therefore needs to pass the `--config` parameter.

## Steps to Reproduce

1. Create a new project, and install `ruff`

   ```shell
   ❯ rye init example-project
   ❯ cd example-project
   ❯ uv venv && source .venv/bin/activate # or `rye sync`
   ❯ uv pip install ruff
   ❯ example-project/.venv/bin/ruff --version
   ruff 0.4.10
   ```

1. In `example-project/pyproject.toml`, configure `per-file-ignores` with relative paths:

   ```toml
   [tool.ruff.lint]
   # The default `rye` project will error on `D103` and `D104` when select = ["ALL"] is used
   select = ["ALL"]

   [tool.ruff.lint.per-file-ignores]
   "./src/example_project/__init__.py" = ["D104"]
   ```

2. Deactivate the `venv`, verify that the `ruff check` command's output is correct from the project's working directory.

   ```
   ❯ deactivate
   ❯ .venv/bin/ruff check --config pyproject.toml src/
   warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
   warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
   example-project/src/example_project/__init__.py:1:5: D103 Missing docstring in public function
   Found 1 error.
   ```

3. Navigate up a directory, then run `ruff check` again

   ```shell
   ❯ cd ..
   ❯ example-project/.venv/bin/ruff check example-project/src/ --config example-project/pyproject.toml
   warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
   warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
   example-project/src/example_project/__init__.py:1:1: D104 Missing docstring in public package
   example-project/src/example_project/__init__.py:1:5: D103 Missing docstring in public function
   Found 2 errors.
   ```

## Environment

- Ruff version: 0.4.10
- OS: macOS 14.5
- Shell: fish

## Possible Solution

Consider adjusting how `ruff` resolves relative paths in the `per-file-ignores` configuration. It may need to use the directory of the `pyproject.toml` file as the base for resolving these paths, rather than the current working directory.


---

_Comment by @MichaReiser on 2024-06-27 05:58_

Thanks for the very detailed issue! 

I had a quick look at the code and I can confirm that paths are always relative to the root directory. 

https://github.com/astral-sh/ruff/blob/72257328591825b7068f4f31a88f8f975ddc538b/crates/ruff_workspace/src/configuration.rs#L486-L519

I'm not entirely sure why the paths are relative to the root and not the configuration. @charliermarsh do you know more about the design decision?

---

_Label `configuration` added by @MichaReiser on 2024-06-27 05:58_

---

_Comment by @mihirsamdarshi on 2024-07-13 03:55_

@MichaReiser If I submitted a patch would that be helpful/accepted? Should I wait for @charliermarsh's input?

---

_Comment by @MichaReiser on 2024-07-13 07:12_

@mihirsamdarshi, I would prefer to hear @charliermarsh's opinion first. I'm not very familiar with that part of Ruff and the configuration schema is kind of complicated. 

---
