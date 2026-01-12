```yaml
number: 11377
title: "`uv` installed incompatible versions of two packages"
type: issue
state: closed
author: emilioMaddalena
labels:
  - question
assignees: []
created_at: 2025-02-10T07:00:04Z
updated_at: 2025-02-10T17:37:05Z
url: https://github.com/astral-sh/uv/issues/11377
synced_at: 2026-01-12T16:00:35Z
```

# `uv` installed incompatible versions of two packages

---

_@emilioMaddalena_

### Summary

**Summary**

I've added both `numpy` and `pmdarima` as dependencies to my project. `uv` decided on "numpy>=2.2.2", which is incompatible with `pmdarima` according to its pyproject.toml file ([link](https://github.com/alkaline-ml/pmdarima/blob/master/pyproject.toml)). This resulted in the latter package not working.

**Platform**

macOS Sonoma v14.6, Macbook air M2

**Version**

uv 0.5.4 (c62c83c37 2024-11-20)

**Python version**

Python 3.11

### Platform

macOS 14.6, M2

### Version

uv 0.5.4 (c62c83c 2024-11-20)

### Python version

Python 3.11

---

_Label `bug` added by @emilioMaddalena on 2025-02-10 07:00_

---

_Label `bug` removed by @konstin on 2025-02-10 08:24_

---

_Label `question` added by @konstin on 2025-02-10 08:24_

---

_Comment by @konstin on 2025-02-10 08:24_

The pyproject.toml entries are build dependencies, the runtime dependencies are compatible with `numpy >=1.21.2`: https://inspector.pypi.io/project/pmdarima/2.0.4/packages/40/e5/78afab229ccdaf6b947036440799dbdf88e2cd632e2f96b81f32de8aa05a/pmdarima-2.0.4-cp311-cp311-macosx_11_0_arm64.whl/pmdarima-2.0.4.dist-info/METADATA#line.39

---

_Comment by @emilioMaddalena on 2025-02-10 17:15_

So how come `uv` arrived at the combination numpy>=2.2.2 and pmdarima>=2.0.4 when I added both packages to my project? 😕 

---

_Comment by @konstin on 2025-02-10 17:17_

What command did you run (and the input files for that command), what versions did you get and what versions did you expect?

---

_Comment by @emilioMaddalena on 2025-02-10 17:22_

I ran `uv add pmdarima` followed by `uv add numpy`.

I got `pmdarima==2.0.4` and `numpy==2.2.2`.

I expected uv to understand that, after adding pmdarima, it had to install an older, compatible version of numpy.

---

_Comment by @konstin on 2025-02-10 17:35_

Checking the install metadata, `pmdarima` declares that is compatible with all numpy versions `>=1.21.2`:

```
$ cat .venv/lib/python3.11/site-packages/pmdarima-2.0.4.dist-info/METADATA | grep Requires-Dist
Requires-Dist: joblib >=0.11
Requires-Dist: Cython !=0.29.18,!=0.29.31,>=0.29
Requires-Dist: numpy >=1.21.2
Requires-Dist: pandas >=0.19
Requires-Dist: scikit-learn >=0.22
Requires-Dist: scipy >=1.3.2
Requires-Dist: statsmodels >=0.13.2
Requires-Dist: urllib3
Requires-Dist: setuptools !=50.0.0,>=38.6.0
Requires-Dist: packaging >=17.1
```

From this perspective, uv has to assume that the latest numpy version will work, it can't tell that there's a runtime incompatibility with numpy 2.0. In this case, either pmdarima needs to add an upper bound, or you can specify that bound yourself, either trough `uv add "numpy<2"`, or by using [constraint-dependencies](https://docs.astral.sh/uv/reference/settings/#constraint-dependencies).

---

_Comment by @emilioMaddalena on 2025-02-10 17:37_

Oh I see, now it makes perfect sense. Thank you for taking the time!

---

_Closed by @emilioMaddalena on 2025-02-10 17:37_

---
