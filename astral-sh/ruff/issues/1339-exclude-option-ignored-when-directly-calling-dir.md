```yaml
number: 1339
title: Exclude option ignored when directly calling dir/file
type: issue
state: closed
author: jhossbach
labels:
  - bug
assignees: []
created_at: 2022-12-22T16:55:33Z
updated_at: 2022-12-22T21:40:16Z
url: https://github.com/astral-sh/ruff/issues/1339
synced_at: 2026-01-12T15:54:41Z
```

# Exclude option ignored when directly calling dir/file

---

_@jhossbach_

MWE:
```toml
[tool.ruff]
exclude = [ "blah" ]
```
`blah/test.py`:
```python
import os
```
Running ruff on `.` returns nothing, but running `ruff ./blah` or `ruff ./blah/test.py` will return the diagnostics (F401) even if blah was excluded. Is this intentional?

---

_Comment by @jhossbach on 2022-12-22 17:14_

*To be more precise: I think this is unwanted because it will mess with parsing the file via stdin and then giving the `stdin-filename`

---

_Comment by @charliermarsh on 2022-12-22 17:15_

Yeah it's intentional. There's a `--force-exclude` flag you can provide on the command-line or in `pyproject.toml` to disable this.

---

_Comment by @charliermarsh on 2022-12-22 17:16_

This is how Black behaves and I think it's pretty reasonable -- if you pass a file directly, we should lint it even if it's marked as an exclusion, since in most cases that's intentional behavior.

---

_Comment by @charliermarsh on 2022-12-22 17:16_

But in some settings, it makes sense to turn that off (for example, with `pre-commit`).

---

_Comment by @charliermarsh on 2022-12-22 17:19_

(And, similarly, it might make sense to always pass that flag in the LSP context.)

---

_Comment by @jhossbach on 2022-12-22 17:19_

In that case I will keep this behavior in `python-lsp-ruff` rather than unintuitively changing it

---

_Closed by @jhossbach on 2022-12-22 17:19_

---

_Comment by @charliermarsh on 2022-12-22 17:20_

I'll probably change `ruff-lsp` to always pass that flag, it seems confusing in the context of an IDE.

---

_Comment by @jhossbach on 2022-12-22 17:32_

I guess you're right, it would also require to add that option to the LSP configuration which should be redundant. 

---

_Comment by @jhossbach on 2022-12-22 18:17_

Sorry to chime in again, but I am still confused about the `--force-exclude`, `--exclude` and the `--stdin-filename` option.
Imagine the following structure
```
.
├── pyproject.toml
└── subdir
    ├── file1.py
    └── file2.py

```
where the `pyproject.toml` contains
```toml
[tool.ruff]
exclude = [ "subdir" ]
```
and `file1.py` and `file2.py` contain
```python
import os
```
It makes sense that `ruff .` returns nothing while `ruff subdir` returns the diagnostics for `file1.py` and `file2.py`, and we can also supress these messages by using `ruff --force-exclude subdir`.
What is confusing to me is why `ruff --force-exclude subdir/file1.py` returns diagnostics **even with** `force-exclude` enabled.
As soon as I change the pyproject.toml file to `exclude = [ "subdir/*" ]`, running `ruff --force-exclude subdir/file1.py` shows no diagnostics again.

Also, the exclude (and force exclude) is still ignored when running **either**
```shell
cat subdir/file1.py | ruff --stdin-filename subdir/file1.py -
cat subdir/file1.py | ruff --stdin-filename subdir/file1.py --force-exclude -
```

My ruff version is v0.0.191.

---

_Reopened by @jhossbach on 2022-12-22 18:17_

---

_Comment by @charliermarsh on 2022-12-22 18:38_

Need to try out the `ruff --force-exclude subdir/file1.py` case to understand what’s going on there, but I think you’re probably right that we’re not respecting exclusions at all when running via stdin (which I misstated above). We need to make code changes to support that, most likely.

---

_Comment by @charliermarsh on 2022-12-22 18:42_

(And no need to apologize, sorry for misleading you above regarding stdin!)

---

_Label `bug` added by @charliermarsh on 2022-12-22 18:59_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-22 18:59_

---

_Comment by @charliermarsh on 2022-12-22 19:51_

The `ruff --force-exclude subdir/file1.py` issue is just sort of a quirk of how the pattern-matching works. If you provide `subdir` as the exclude, then we exclude any file or directory that ends in `subdir`. If you provide `subdir` as the folder to check on the command-line, then we exclude it due to that match and avoid recursing into it. If you provide `subdir/file1.py`, we never actually check `subdir`, just `file1.py` and `subdir/file1.py`, which is why `subdir/*` actually _does_ work.

I think this can be improved, but need to consider what the right fix is...


---

_Comment by @jhossbach on 2022-12-22 20:06_

How about a gitignore-like style? It would mean that `foo/` would exclude a dir and everything beneath, while `foo/*` would match everything directly beneath (so `foo/bar/test.py`) would not be excluded (of course we can relax this part).
`subdir/file1.py` would match this file and this file only, while `file1.py` would match `file1.py` only in the first directory, while `**file1.py` matches `file1.py` in every directory.

---

_Comment by @charliermarsh on 2022-12-22 20:09_

I accidentally pushed it directly but the fix is (hopefully) in https://github.com/charliermarsh/ruff/commit/451047c30ddcac143f3d5f9958e0c8bcd5744a9c. The idea is that if you pass `subdir/file.py`, and `force-exclude` is set, we actually need to ensure that it's not in an excluded directly -- so we just do that pass before walking.

---

_Comment by @jhossbach on 2022-12-22 21:32_

Seems to work :+1: I'll open up a new issue for the extension to when passing the file through stdin.

---

_Closed by @charliermarsh on 2022-12-22 21:40_

---
