```yaml
number: 1692
title: "`--index-url` from `requirements.txt` is ignored"
type: issue
state: closed
author: framillien
labels:
  - bug
assignees: []
created_at: 2024-02-19T13:58:49Z
updated_at: 2024-02-20T19:08:57Z
url: https://github.com/astral-sh/uv/issues/1692
synced_at: 2026-01-10T05:40:31Z
```

# `--index-url` from `requirements.txt` is ignored

---

_Issue opened by @framillien on 2024-02-19 13:58_

`--index-url` is ignored in `requirements.txt`, no issue with `--extra-index-url` parameter.

Look like it's related to the way IndexUrl argument is used. This argument is always initialized with a value [(here)](https://github.com/astral-sh/uv/blob/main/crates/uv/src/main.rs#L530) and `combine` always keep the previous value if present [(here)](https://github.com/astral-sh/uv/blob/main/crates/distribution-types/src/index_url.rs#L193).

If I invert the expression in `combine`, my `--index-url` in `requirements.txt` is correctly used; but this is maybe not the expected way to fix it because command line parameter (if really defined) should take precedence ?

This affect both `pip install` and `pip  compile` commands.

Fail with this `requirements.txt`:
```
--index-url https://my-pip-index.org/artifactory/api/pypi/pypi-custom-release/simple/

my-custom-package
```

Work with:
```
--extra-index-url https://my-pip-index.org/artifactory/api/pypi/pypi-custom-release/simple/

my-custom-package
```

With both commands:
```
uv -v --no-cache pip install requirements.txt
uv -v --no-cache pip compile requirements.in
```

To set the `--index-url`, the current workaround is to always set in on command line like:
```
uv -v --no-cache pip install --index-url https://my-pip-index.org/artifactory/api/pypi/pypi-custom-release/simple/ requirements.txt
```

---

_Comment by @charliermarsh on 2024-02-19 14:28_

Will take a look, thanks!

---

_Label `bug` added by @charliermarsh on 2024-02-19 14:28_

---

_Comment by @framillien on 2024-02-19 14:43_

(description edited because  it's more related to arguments default values rather than `IndexLocations` default values)

---

_Comment by @PetterS on 2024-02-19 15:30_

Is it possible to add a private PyPI in the `pyproject.toml` in a standardized way that uv understands? That is quite important in many corporate settings.  `poetry` supports this via the non-standard `[[tool.poetry.source]]` so that works well for corporations. 

---

_Comment by @zanieb on 2024-02-19 15:55_

Not yet, but we will support persistent configuration of registries in the future.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-19 20:01_

---

_Comment by @framillien on 2024-02-19 21:47_

Thanks for the quick fix

---

_Closed by @charliermarsh on 2024-02-20 00:02_

---

_Comment by @PetterS on 2024-02-20 19:08_

> Not yet, but we will support persistent configuration of registries in the future.

@zanieb is there a tracking issue for that feature I can subscribe to?

---
