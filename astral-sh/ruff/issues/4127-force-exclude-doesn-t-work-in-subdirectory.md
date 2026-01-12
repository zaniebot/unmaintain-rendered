```yaml
number: 4127
title: "force-exclude doesn't work in subdirectory"
type: issue
state: open
author: brandonchinn178
labels:
  - bug
assignees: []
created_at: 2023-04-27T00:25:33Z
updated_at: 2024-02-16T12:06:26Z
url: https://github.com/astral-sh/ruff/issues/4127
synced_at: 2026-01-12T15:54:44Z
```

# force-exclude doesn't work in subdirectory

---

_@brandonchinn178_

When a python project is in a subdirectory (e.g. the `pyproject.toml` is in `subdir/pyproject.toml`), `force-exclude` no longer excludes files passed on the command line.

This is an issue when using ruff in a pre-commit hook. I could have the pre-commit hook cd into the subdirectory, but then I'd need to strip the prefix on all arguments

## Repro

Create the following files:
```toml
# subdir/pyproject.toml
[tool.ruff]
select = ["F"]
extend-exclude = ["skip.py"]
force-exclude = true
```
```py
# subdir/skip.py
import math
```
```py
# subdir/foo.py
import math
```

1. `python3 -m venv venv`
2. `venv/bin/pip install ruff`
3. `venv/bin/ruff subdir/`
    * ✅  Finds `subdir/foo.py`
4. `venv/bin/ruff subdir/*.py`
    * 💥  Finds `subdir/foo.py` and `subdir/skip.py`
5. `(cd subdir && ../venv/bin/ruff *.py)`
    * ✅  Finds `subdir/foo.py`

---

_Comment by @charliermarsh on 2023-04-27 01:28_

The root cause here is that `force-exclude` is one of a handful of options that are really meant to be provided as command-line arguments, but that we do respect when in a `pyproject.toml` _in the current working directory_. Those include:

```rust
// TODO(charlie): Captured in pyproject.toml as a default, but not part of `Settings`.
pub cache_dir: Option<PathBuf>,
pub fix: Option<bool>,
pub fix_only: Option<bool>,
pub force_exclude: Option<bool>,
pub format: Option<SerializationFormat>,
pub show_fixes: Option<bool>,
pub update_check: Option<bool>,
...
```

`force_exclude` is one that we could perhaps support more generally, I'd need to poke around at it.


---

_Comment by @charliermarsh on 2023-04-27 01:29_

(I might suggest passing `--force-exclude` explicitly in your pre-commit hook, that's what we suggest for `pre-commit` specifically IIRC, but of course I'm not totally familiar with your setup and whether that's acceptable.)

---

_Label `bug` added by @charliermarsh on 2023-04-27 01:29_

---

_Comment by @brandonchinn178 on 2023-04-27 01:33_

Ah thanks! Didn't think setting via CLI flag would be any different than via pyproject.toml. That works!

---

_Comment by @charliermarsh on 2023-04-27 02:15_

Yeah, it's just those few flags that I linked above. The issue, in most cases, is that they're only defined as top-level controls, and not recursively (e.g., we don't support mixing output formats between files in different directories in a single invocation).

---

_Assigned to @charliermarsh by @charliermarsh on 2023-04-29 04:16_

---

_Comment by @laszlo-hypergolic on 2024-02-15 16:37_

Was this changed/fixed since this thread? 

I just added `force-exclude = true` to `pyproject.toml` and worked in the same situation (pre-commit hooks).

---

_Comment by @dhruvmanila on 2024-02-16 07:11_

> worked in the same situation

Are you saying that it worked as expected or did you run into the same issue as mentioned in this thread?

---

_Comment by @laszlo-hypergolic on 2024-02-16 12:06_

I have a `config.py` that imports a lot of packages that are not used in the file (I use this to declutter Jupyter notebooks).
Ruff was complaining about it. I added the file (and then the dir) to `exclude = ["notebooks"]` then to `extend-exclude = ["notebooks"]` then came to this thread, added `force-exclude = true` 

then run the command. I thought that it works and then I made the comment but when I checked it turned out that ruff just forced --fix the file and deleted the unused imports. (I then added `#noqa` to the top of the file and that made my problem go away but of course didn't solve the problem that `exclude=` doesn't seem to be working.

---
