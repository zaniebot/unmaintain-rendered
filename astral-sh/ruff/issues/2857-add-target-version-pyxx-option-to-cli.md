```yaml
number: 2857
title: "Add `--target-version pyXX` option to CLI"
type: issue
state: closed
author: kdeldycke
labels: []
assignees: []
created_at: 2023-02-13T14:45:51Z
updated_at: 2023-05-06T12:49:23Z
url: https://github.com/astral-sh/ruff/issues/2857
synced_at: 2026-01-10T11:09:45Z
```

# Add `--target-version pyXX` option to CLI

---

_Issue opened by @kdeldycke on 2023-02-13 14:45_

This is a feature request to add a `--target-version pyXX` to ruff.

The idea is to mimick Black by allowing any subset of the following values:

- `--target-version py37`
- `--target-version py38`
- `--target-version py39`
- `--target-version py310`
- `--target-version py311`
- ...

With the idea that [you should include all Python versions that you want your code to run under (as Black does)](https://github.com/psf/black/issues/751#issuecomment-473066811).

This exact format is [also what `blacken-docs` CLI supports](https://github.com/adamchainz/blacken-docs/blob/cd4e60f68cbac1d7fd2a1586c3a8c4f0fbd196c7/src/blacken_docs/__init__.py#L263-L271).

On a practical level, this will help me a lot as I already [generates these flags in my build pipeline](https://github.com/kdeldycke/workflows/blob/355d4ed18b8133660e5a2cda13f58602f0999fa1/.github/metadata.py#L337-L386) for Black and blacken-docs. These are [generated dynamically by inspecting the `pyproject.toml`](https://github.com/kdeldycke/workflows/blob/355d4ed18b8133660e5a2cda13f58602f0999fa1/.github/metadata.py#L325-L335) of my projects.

---

_Comment by @kdeldycke on 2023-02-13 14:49_

For the record, this is adjacent to: https://github.com/charliermarsh/ruff/issues/2039 and https://github.com/charliermarsh/ruff/issues/2519

---

_Comment by @charliermarsh on 2023-02-13 14:50_

Hey thanks for filing! This exact API actually _is_ available on the CLI, I just hid it by accident in a prior commit 🤦 

---

_Comment by @kdeldycke on 2023-02-13 14:56_

Oh wow that's perfect! And incredibly fast!

I can test  #2859 as soon as a release is publish.

---

_Comment by @charliermarsh on 2023-02-13 14:59_

It _should_ actually work even with existing versions (`ruff check /path/to/file.py --target-version py311`), it just won't show up in `--help`. If you see otherwise, let me know!

---

_Closed by @charliermarsh on 2023-02-13 15:04_

---

_Comment by @kdeldycke on 2023-02-13 15:05_

Works indeed!
```shell-session
$ ruff --version
ruff 0.0.245

$ ruff check --fix-only --exit-zero --diff --target-version 311 ./.github/metadata.py
error: invalid value '311' for '--target-version <TARGET_VERSION>': Unknown version: 311 (try: "py37")

For more information, try '--help'.

$ ruff check --fix-only --exit-zero --diff --target-version py311 ./.github/metadata.py
```

No rush then to publish a new version of ruff.

Feel free to close this issue! :)

And thanks a lot for ruff, I [played with it today](https://twitter.com/kdeldycke/status/1625120220636495872) and [was able to get rid of `iSort`, `Pyupgrade`, `Pylint`, `Pycln` and `Pydocstyle`](https://github.com/kdeldycke/workflows/compare/v2.7.6...main#diff-93681562069e3d6bd247d402c10c794bf75baa6e1ff97676b50fda96343e251d)!

---

_Comment by @charliermarsh on 2023-02-13 15:06_

Amazing, thanks for giving Ruff a try :)

---

_Comment by @kdeldycke on 2023-02-13 15:06_

Just FYI, it doesn't supports multiple value yet, but that's a detail:
```shell-session
❯ ruff check --fix-only --exit-zero --target-version py38 --target-version py311 ./.github/metadata.py
error: the argument '--target-version <TARGET_VERSION>' cannot be used multiple times

Usage: ruff check [OPTIONS] [FILES]...

For more information, try '--help'.
```

---
