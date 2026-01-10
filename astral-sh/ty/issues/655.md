```yaml
number: 655
title: "Specialized `@overload` not respected"
type: issue
state: closed
author: galah92
labels: []
assignees: []
created_at: 2025-06-14T10:04:27Z
updated_at: 2025-06-17T02:24:40Z
url: https://github.com/astral-sh/ty/issues/655
synced_at: 2026-01-10T02:08:20Z
```

# Specialized `@overload` not respected

---

_Issue opened by @galah92 on 2025-06-14 10:04_

### Summary

According to [this](https://github.com/matplotlib/matplotlib/blob/f6b77d286fd7617be24c6b8eda21cac4c0056293/lib/matplotlib/figure.pyi#L92) I expect `ty` to pickup the return type is `Axes3D` and not `Axes.` However:

```bash
➜  Downloads uv init tyoverload
Initialized project `tyoverload` at `/Users/galah92/Downloads/tyoverload`
➜  Downloads cd tyoverload
➜  tyoverload git:(main) ✗ vi main.py
➜  tyoverload git:(main) ✗ uv add matplotlib ty
Resolved 13 packages in 1.27s
Prepared 7 packages in 4.89s
Installed 12 packages in 108ms
 + contourpy==1.3.2
 + cycler==0.12.1
 + fonttools==4.58.4
 + kiwisolver==1.4.8
 + matplotlib==3.10.3
 + numpy==2.3.0
 + packaging==25.0
 + pillow==11.2.1
 + pyparsing==3.2.3
 + python-dateutil==2.9.0.post0
 + six==1.17.0
 + ty==0.0.1a10
➜  tyoverload git:(main) ✗ cat main.py
import matplotlib.pyplot as plt


fig = plt.figure()
ax = fig.add_subplot(projection='3d')
ax.set_zlim(0, 1)
➜  tyoverload git:(main) ✗ uv run ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-attribute]: Type `Axes` has no attribute `set_zlim`
 --> main.py:6:1
  |
4 | fig = plt.figure()
5 | ax = fig.add_subplot(projection='3d')
6 | ax.set_zlim(0, 1)
  | ^^^^^^^^^^^
  |
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.1-alpha.10 (15bae14c2 2025-06-13)

---

_Comment by @carljm on 2025-06-17 02:24_

Thanks for the report!

This is because the latest released matplotlib (3.10.3) does not yet include the additional overload for `Axes3D`: https://github.com/matplotlib/matplotlib/blob/8b8272945441b74d5dd7b5815c88a49608b8926d/lib/matplotlib/figure.pyi#L88

If I instead `uv add "matplotlib @ git+https://github.com/matplotlib/matplotlib"` to install the latest matplotlib from github main branch, then your code works without errors.

Closing since this doesn't appear to be a bug in ty.

---

_Closed by @carljm on 2025-06-17 02:24_

---
