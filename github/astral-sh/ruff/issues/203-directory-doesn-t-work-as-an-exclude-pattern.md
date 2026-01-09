---
number: 203
title: "`directory/*` doesn't work as an exclude pattern"
type: issue
state: closed
author: Jackenmen
labels:
  - bug
assignees: []
created_at: 2022-09-15T15:58:38Z
updated_at: 2023-02-03T18:28:28Z
url: https://github.com/astral-sh/ruff/issues/203
synced_at: 2026-01-07T13:12:14-06:00
---

# `directory/*` doesn't work as an exclude pattern

---

_Issue opened by @Jackenmen on 2022-09-15 15:58_

I tested out the current main branch and the exclusion globs seem somewhat broken.
For example, with either of these configurations:
```
[tool.ruff]
extend-exclude = [
    "directory/*",
]
```
```
[tool.ruff]
extend-exclude = [
    "directory/file.py",
]
```
Running `ruff .` doesn't end up with ruff excluding `directory/file.py`.

`isort` with `skip_glob` set to the same list as ruff's `extend-exclude` works fine (which I'm mentioning because you said in the PR that you wanted to mimic isort's behavior here).


---

_Referenced in [astral-sh/ruff#202](../../astral-sh/ruff/pulls/202.md) on 2022-09-15 15:59_

---

_Comment by @charliermarsh on 2022-09-15 16:08_

I think with the current strategy, this needs to be `"directory"` or `"file.py"`.


---

_Comment by @charliermarsh on 2022-09-15 16:10_

I ended up mirroring the Flake8 behavior which is a bit different than isort.

---

_Comment by @Jackenmen on 2022-09-15 16:13_

Hmm, not being able to match against the whole path (`directory/file.py`) seems a bit too limiting.

---

_Comment by @charliermarsh on 2022-09-15 16:17_

Yeah, that's fixable, can do...

---

_Comment by @charliermarsh on 2022-09-15 16:18_

`directory/file.py` is actually supposed to work (it works in Flake8 IIUC), I just haven't implemented it.

---

_Label `bug` added by @charliermarsh on 2022-09-15 16:18_

---

_Added to milestone `Release 0.1.0` by @charliermarsh on 2022-09-15 16:18_

---

_Comment by @Jackenmen on 2022-09-15 16:28_

After testing, both the patterns I listed in the issue description work with flake8.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-09-15 22:04_

---

_Referenced in [astral-sh/ruff#209](../../astral-sh/ruff/pulls/209.md) on 2022-09-16 01:40_

---

_Closed by @charliermarsh on 2022-09-16 01:40_

---

_Comment by @charliermarsh on 2022-09-16 01:42_

Okay, you should now be able to do:

- `directory` (to match any directory named `directory`)
- `./directory/file.py` (to match that file specifically)

Unlike other tools, I'm explicitly requiring that relative paths are written with the relative prefix (`./`). Otherwise, the path will effectively only be matched against the `basename`.


---

_Comment by @amotl on 2022-09-16 08:10_

Hi,

thanks for this improvement. However, with `ruff 0.0.37`, this syntax worked:
```toml
exclude = [
  "tests/etc/functions_bad.py"
]
```

Now, with `ruff 0.0.39`, I would have to write
```toml
exclude = [
  "./tests/etc/functions_bad.py"
]
```

Is this correct? It actually just croaked at [1]. Maybe it can be made a bit more lenient?

With kind regards,
Andreas.

[1] https://github.com/jpmens/mqttwarn/actions/runs/3066290569

---

_Comment by @amotl on 2022-09-16 08:11_

> Is this correct? It actually just croaked at [1]. Maybe it can be made a bit more lenient?

Oh I see:

> Unlike other tools, I'm explicitly requiring that relative paths are written with the relative prefix (`./`). Otherwise, the path will effectively only be matched against the basename.

Adjusted with https://github.com/jpmens/mqttwarn/commit/fad50af912e.


---

_Referenced in [astral-sh/ruff#210](../../astral-sh/ruff/issues/210.md) on 2022-09-16 08:38_

---

_Comment by @charliermarsh on 2022-09-16 09:02_

Yeah I think this could be better -- I'll try it here: #211.

---

_Referenced in [astral-sh/ruff#234](../../astral-sh/ruff/issues/234.md) on 2022-09-20 16:03_

---
