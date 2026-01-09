---
number: 11806
title: "Problem with `opencv-python-headless` installed via uv but not via pip"
type: issue
state: closed
author: Yc-Chen
labels:
  - question
assignees: []
created_at: 2025-02-26T17:53:39Z
updated_at: 2025-03-02T00:48:11Z
url: https://github.com/astral-sh/uv/issues/11806
synced_at: 2026-01-07T13:12:18-06:00
---

# Problem with `opencv-python-headless` installed via uv but not via pip

---

_Issue opened by @Yc-Chen on 2025-02-26 17:53_

### Summary

I created a reproduction repository here: https://github.com/Yc-Chen/uv-opencv-error

The details of reproduction steps are written in the README.

In short, inside a docker image, when the package `opencv-python-headless` is installed by uv, the `import cv2` gives out an error; but if this package is installed by pip, it works.

Maybe a related issue: https://github.com/astral-sh/uv/issues/10708

### Platform

macOS 13, Intel

### Version

uv 0.5.31 (e38ac4900 2025-02-12)

### Python version

Python 3.12

---

_Label `bug` added by @Yc-Chen on 2025-02-26 17:53_

---

_Renamed from "Problem with opencv-python-headless installed via uv but not via pip" to "Problem with `opencv-python-headless` installed via uv but not via pip" by @Yc-Chen on 2025-02-26 19:21_

---

_Comment by @charliermarsh on 2025-03-01 01:59_

I think the problem is:

```diff
diff --git a/mypackage/pyproject.toml b/mypackage/pyproject.toml
index 599974c..81185a6 100644
--- a/mypackage/pyproject.toml
+++ b/mypackage/pyproject.toml
@@ -4,7 +4,7 @@ version = "0.1.0"
 description = "Add your description here"
 requires-python = ">=3.11"
 dependencies = [
-    "opencv-python>=4.11.0.86",
+    "opencv-python-headless>=4.11.0.86",
 ]
```

You have a dependency on `opencv-python` in the child package. So we're installing `opencv-python`, and _that_ version of the package requires system dependencies. You should use the headless version everywhere, I think? If I apply that change, I can `import cv2` in the container.

---

_Label `bug` removed by @charliermarsh on 2025-03-01 01:59_

---

_Label `question` added by @charliermarsh on 2025-03-01 01:59_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-01 02:00_

---

_Comment by @Yc-Chen on 2025-03-01 15:31_

Ah, I initially suspected the child package may have caused dependency conflict so I included it. But now I just removed `mypackage` [there](https://github.com/Yc-Chen/uv-opencv-error/commit/ddbe361af4df0b282fe2b4b765c4422f668befb9) and you will see the same error: `import cv2` fails with default container, but not with `pip.Dockerfile`.

---

_Comment by @charliermarsh on 2025-03-01 18:40_

You need to re-run uv lock, right? You didn’t update the lockfile.

---

_Comment by @Yc-Chen on 2025-03-01 22:08_

oh, you are right...although I still don't get it. Why is it like this? I tried out more combinations and they are:

| tag | root package includes       |  `mypackage` includes                  |  Dockerfile  | pip.Dockerfile |
|---|-------------------|-------------------------------------------|-------------------------------------------------------|-----|
| v2-no-error | `opencv-python-headless`    | -   | No error                       | No error |
| v1 | `opencv-python-headless`, `mypackage`  | `opencv-python`       | ImportError: libGL.so.1: cannot open shared object file | No error |
| v3 | `mypackage` | `opencv-python` |  ImportError: libGL.so.1: cannot open shared object file | No error |
| v7  | `mypackage`, `opencv-python` | `opencv-python` | ImportError: libGL.so.1: cannot open shared object file  | No error | 
| v4-no-error | `mypackage` | `opencv-python-headless` | No error | No error |
| v5 | `opencv-python`, `mypackage` | `opencv-python-headless` | No error | No error |
| v6  | `mypackage`, `opencv-python-headless` | `opencv-python-headless` | No error | No error | 

I even explored the case where you have 2 child packages where one contains `opencv-python` and the other one contains `opencv-python-headless`. But result is similar.

My conclusion is, whenever a child package includes `opencv-python`, the best thing to do is to avoid installing `opencv-python` and force install `opencv-python-headless`. In dockerfile:

```diff
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
---    uv sync --frozen --no-install-project --no-dev
+++    uv sync --frozen --no-install-project --no-dev --no-install-package opencv-python

...
RUN --mount=type=cache,target=/root/.cache/uv \
---    uv sync --frozen --no-dev
+++    uv sync --frozen --no-dev --no-install-package opencv-python && \
+++    uv pip install opencv-python-headless

```

---

_Comment by @charliermarsh on 2025-03-02 00:48_

I don't know why pip works in some of those cases but you definitely don't want to mix them. If you install both packages, I'd consider it "random" whether the import works or not. It would just depend on install order.

---

_Closed by @charliermarsh on 2025-03-02 00:48_

---

_Referenced in [astral-sh/uv#13437](../../astral-sh/uv/pulls/13437.md) on 2025-05-13 20:55_

---

_Referenced in [astral-sh/uv#15357](../../astral-sh/uv/issues/15357.md) on 2025-08-18 16:36_

---
