```yaml
number: 1218
title: Support formatting jupyter notebooks
type: issue
state: closed
author: konstin
labels:
  - core
  - help wanted
assignees: []
created_at: 2022-12-12T17:15:11Z
updated_at: 2023-06-20T05:44:09Z
url: https://github.com/astral-sh/ruff/issues/1218
synced_at: 2026-01-10T11:09:43Z
```

# Support formatting jupyter notebooks

---

_Issue opened by @konstin on 2022-12-12 17:15_

[black](https://github.com/psf/black) supports formatting jupyter notebook with the jupyter extra `black[jupyter]`. If installed like this, it formats jupyter notebooks just like python files. It would be nice if ruff could similarly lint and fix jupyter notebooks.

Steps to implement this:

- [x] Read Jupyter notebooks, [build a mapping between cells and concatenated code](https://github.com/charliermarsh/ruff/blob/cab65b25da8aff6fb9cb539b55345aae64a835f0/crates/ruff/src/jupyter/notebook.rs#L136-L187) and and print lint errors with cell ids. The code is in [jupyter/notebooks.rs](https://github.com/charliermarsh/ruff/blob/main/crates/ruff/src/jupyter/notebook.rs).
- [x] Apply fixes to concatenated code and map back to cells
- [x] Write back jupyter notebooks ensuring that if there are no changes the json is identical (roundtrip support). You might need to extend or replace the current [schema](https://github.com/charliermarsh/ruff/blob/595cd065f3a002a4301c41f005122a8806a3925f/crates/ruff/src/jupyter/schema.rs#L23) or drop some of the schema for just `serde_json::Value`. See also the [black implementation](https://github.com/psf/black/blob/69ca0a4c7a365c5f5eea519a90980bab72cab764/src/black/__init__.py#L1017-L1046).
- [x] Test roundtrip support with a bunch of different notebooks found in the wild, ideally from different generators (e.g. jupyter notebook and pycharm) and different schema versions.
- [ ] Remove `jupyter_notebook` feature and include `.ipynb` files in ruff by default
- [ ] Optional: Check if there a any lints that should be off for jupyter notebook, e.g. "no print" or isort rules might not make sense

---

_Label `enhancement` added by @charliermarsh on 2022-12-12 17:16_

---

_Comment by @charliermarsh on 2022-12-26 04:02_

I assume Ruff would work with [nbQA](https://github.com/nbQA-dev/nbQA), but I too would like it to support Jupyter out-of-the-box. Looking into it...

---

_Comment by @charliermarsh on 2022-12-28 02:18_

Ruff was actually integrated into [nbQA](https://github.com/nbQA-dev/nbQA/pull/773) in the latest release, so you can now run (e.g.):

```
❯ nbqa ruff Untitled.ipynb
Untitled.ipynb:cell_1:2:5: F841 Local variable `x` is assigned to but never used
Untitled.ipynb:cell_2:1:1: E402 Module level import not at top of file
Untitled.ipynb:cell_2:1:8: F401 `os` imported but unused
Found 3 error(s).
1 potentially fixable with the --fix option.
```

(Would still like to have a first-party integration at some point.)

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:14_

---

_Label `core` added by @charliermarsh on 2022-12-31 18:14_

---

_Comment by @blakeNaccarato on 2023-03-10 03:21_

See a parallel effort over in the Ruff VSCode extension, where LSP support for analyzing entire Jupyter notebooks seems blocked by `pygls` still being on LSP v3.16. It could be a shorter road for the Ruff LSP implementation to handle entire notebook formatting, if effort can be concentrated over at `pygls` to get LSP 3.17 out. See [my comment](https://github.com/charliermarsh/ruff-vscode/issues/51#issuecomment-1463190089) in the other thread for more detail and other Issue links.

It would be nice to have native support in Ruff as well! But in the short run, it looks like LSP support may have less resistance.

---

_Comment by @konstin on 2023-05-11 11:58_

The current implementation can lint jupyter files, but it can't apply fixes yet or write them back to file. I've written instructions in the top comment and can mentor if someone wants to tackle this.

---

_Label `help wanted` added by @konstin on 2023-05-12 11:29_

---

_Comment by @sladyn98 on 2023-05-16 21:25_

@konstin I could take this on 

---

_Comment by @charliermarsh on 2023-05-17 01:19_

@sladyn98 - Sorry for the churn, I already chatted with @dhruvmanila about taking this one on!

---

_Comment by @dhruvmanila on 2023-05-17 03:01_

@charliermarsh you can assign this to me to avoid any future confusion :)

---

_Assigned to @dhruvmanila by @charliermarsh on 2023-05-17 03:08_

---

_Closed by @dhruvmanila on 2023-06-12 14:14_

---

_Comment by @dhruvmanila on 2023-06-12 15:43_

Sorry, not yet completed.

---

_Reopened by @dhruvmanila on 2023-06-12 15:43_

---

_Comment by @charliermarsh on 2023-06-12 15:47_

What's the outstanding work here? Testing?

---

_Comment by @dhruvmanila on 2023-06-12 16:06_

* Roundtrip support
* End-to-end testing support

---

_Comment by @dhruvmanila on 2023-06-20 05:43_

Meta issue: #5188

---

_Closed by @dhruvmanila on 2023-06-20 05:44_

---
