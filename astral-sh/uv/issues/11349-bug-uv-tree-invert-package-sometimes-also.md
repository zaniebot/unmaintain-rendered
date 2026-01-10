---
number: 11349
title: "BUG: `uv tree --invert --package ...` sometimes also displays packages I didn't ask for"
type: issue
state: open
author: neutrinoceros
labels:
  - bug
assignees: []
created_at: 2025-02-09T09:03:05Z
updated_at: 2025-02-10T08:53:38Z
url: https://github.com/astral-sh/uv/issues/11349
synced_at: 2026-01-10T01:25:04Z
---

# BUG: `uv tree --invert --package ...` sometimes also displays packages I didn't ask for

---

_Issue opened by @neutrinoceros on 2025-02-09 09:03_

### Summary

When I run `uv tree --invert --package ...` against astropy, I sometimes get more packages to show up in the displayed tree than I asked for, which seems like a bug.
This is obvious when requesting a single package as root and limiting the depth to 0.
```
❯ git clone https://github.com/astropy/astropy
❯ cd astropy
❯ uv tree --invert --package setuptools -d 0 --no-config
Resolved 166 packages in 8ms
asdf-astropy v0.7.0
setuptools v75.8.0
❯ uv tree --invert --package six -d 0 --no-config
Resolved 166 packages in 8ms
asdf-astropy v0.7.0
six v1.17.0
```

In all cases I found that reproduce this problem, `asdf-astropy` seems to be the one package that show up uninvited. However, it may or may not show up depending on the package I ask for:
```
❯ uv tree --invert --package numpy -d 0
Resolved 166 packages in 1ms
numpy v2.2.2
```

The output seems consistent (no randomness I can see) for any given command, however I don't have a clear idea why `--package numpy` works and `--package six` doesn't. My experiments suggest it has nothing to do with numpy being a direct and hard dependency to astropy, as I also hit the bug with `--package astropy-iers-data`
```
❯ uv tree --invert --package astropy-iers-data -d 0
Resolved 166 packages in 1ms
asdf-astropy v0.7.0
astropy-iers-data v0.2025.2.3.0.32.42
```

I wasn't able to reproduce this with a simpler package, though I admit I only tried a bottom-up approach; I did not attempt to simplify astropy's dependencies.

### Platform

macOS 15.3 arm64

### Version

uv 0.5.29 (ca73c4754 2025-02-05)

### Python version

Python 3.12.9

---

_Label `bug` added by @neutrinoceros on 2025-02-09 09:03_

---

_Comment by @charliermarsh on 2025-02-09 18:11_

Thanks, we'll take a look.

---

_Comment by @charliermarsh on 2025-02-09 18:23_

I think this is because there's a circular dependency in the graph, whereby `asdf-astropy` depends on `astropy`, but `astropy` depends on `asdf-astropy` -- at least, as far as I can tell? So, like, it's _trying_ to show you that `setuptools` is required by `astropy-sphinx-theme` which is required by `sphinx-astropy` which is required by `astropy`, but then `astropy` is both required by and requires `asdf-astropy`, which is hard to display.


---

_Comment by @neutrinoceros on 2025-02-09 19:10_

> asdf-astropy depends on astropy, but astropy depends on asdf-astropy -- at least, as far as I can tell?

[I can confirm it does](https://github.com/astropy/asdf-astropy/blob/462068492628f01901cb7f6f3b4f2fb46de3945f/pyproject.toml#L24), and sorry for not thinking of this earlier !


> I think this is because there's a circular dependency in the graph (...) which is hard to display.

Granted. Maybe the cycle could be indicated as `...`, inspired by the Python repr for self-referential objects, as in
```py
>>> L = []
>>> L.append(L)
>>> print(L)
[[...]]
```
?

---

_Comment by @JadenMajid on 2025-02-10 08:53_

Hi, can i work on this?

---

_Referenced in [astral-sh/uv#15907](../../astral-sh/uv/issues/15907.md) on 2025-09-17 11:51_

---
