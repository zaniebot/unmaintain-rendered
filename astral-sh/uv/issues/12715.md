```yaml
number: 12715
title: Is UV_SYSTEM_PYTHON variable fully respected?
type: issue
state: closed
author: houtianze
labels:
  - bug
assignees: []
created_at: 2025-04-07T14:26:57Z
updated_at: 2025-04-07T16:13:59Z
url: https://github.com/astral-sh/uv/issues/12715
synced_at: 2026-01-10T03:41:47Z
```

# Is UV_SYSTEM_PYTHON variable fully respected?

---

_Issue opened by @houtianze on 2025-04-07 14:26_

### Summary

I'm not entirely sure if it's a bug in `uv` or some misconfigured in Google Colab, but here is the Colab notebook that reproduced this:
https://colab.research.google.com/drive/17Ev3IAHypidXVbOSKRBH3sp9tWEnL_Jn

Google Colab somehow sets the `UV_SYSTEM_PYTHON ` env var to `true`. If you run the code cell one by one (or just view the existing output), you will find the `uv sync` seems to not have installed the `spacy` package into the `.venv` directory, and running the system python directly can access this package, but when running `uv run python ...`, it can't find it. I don't know what is going on behind the scene, but this behavior doesn't look right too me.


### Platform

Linux x86_64

### Version

0.6.12

### Python version

3.13

---

_Label `bug` added by @houtianze on 2025-04-07 14:26_

---

_Comment by @charliermarsh on 2025-04-07 14:27_

`UV_SYSTEM_PYTHON` doesn't affect the project APIs (`uv sync`, `uv run`, etc.). I believe it only affects the `uv pip` API.

---

_Comment by @charliermarsh on 2025-04-07 14:28_

The project APIs (`uv sync`, `uv run`, etc.) can instead be configured via `UV_PROJECT_ENVIRONMENT`.

---

_Comment by @houtianze on 2025-04-07 15:53_

Thansk for the quick response.

After further inspection, it's should be due to `misaki[zh]` not doesn't have the `spacy` package as its dependency which I assume it does. I got confused as why `spacy` is not installed to `.venv` in this case but `uv pip` still sees it (the system one). In this case, `uv` behaves correctly per your explanation (it only affect `uv pip`), thus this is not a bug. Closing it now, thanks again.

---

_Closed by @houtianze on 2025-04-07 15:53_

---
