```yaml
number: 8671
title: "Using `uv` w/ Databricks"
type: issue
state: closed
author: soneill-pure
labels:
  - question
assignees: []
created_at: 2024-10-29T17:13:05Z
updated_at: 2025-07-31T11:56:51Z
url: https://github.com/astral-sh/uv/issues/8671
synced_at: 2026-01-12T15:59:31Z
```

# Using `uv` w/ Databricks

---

_@soneill-pure_

Has anyone had success using `uv` with Databricks? I'm loving `uv` for local project development and I'd like the ability to quickly offload heavy ML compute jobs to Databricks which would involve replicating my local virtual env on Databricks. I'd prefer to use `uv` to replicate the env for speed and parity. My initial thoughts are to create a [custom docker container](https://docs.databricks.com/en/compute/custom-containers.html) that uses `uv` instead of vanilla `virtualenv` & `pip` that they currently use in their [base images](https://github.com/databricks/containers/blob/master/ubuntu/python/Dockerfile).


---

_Comment by @soneill-pure on 2024-10-29 21:18_

Here is one workaround that worked for me in Databricks

```python
%pip install uv

# need to use non-default virtual env location for `uv` on Databricks. will piggyback on the existing virtual env Databricks has setup automatically.
# python version specified in pyproject.toml should match Databricks Runtime python version (no known workaround).
import os
os.environ["UV_PROJECT_ENVIRONMENT"] = os.environ["VIRTUAL_ENV"]

# reproduce `uv` environment on Databricks.
%sh uv sync --locked --no-editable
```

---

_Label `question` added by @samypr100 on 2024-10-31 03:39_

---

_Comment by @samypr100 on 2024-10-31 03:44_

I haven't tried this yet, but might do so eventually. Your workaround seems correct to me.
You may avoid the issue with editable by passing `--no-editable` to uv sync, although it may depend on your pyproject.toml setup.

---

_Comment by @soneill-pure on 2024-10-31 05:46_

Thanks for the `--no-editable` tip. That worked for my project.

---

_Comment by @zanieb on 2024-10-31 13:28_

Nice, that seems reasonable. Let us know if you have more questions.

---

_Closed by @zanieb on 2024-10-31 13:28_

---

_Comment by @astrojuanlu on 2024-11-11 12:32_

Similar to #4379

---

_Comment by @frbelotto on 2025-07-29 13:23_

Sorry do reopen it, but does it still work?
I am really struggling to using UV as package manager for my project on databricks

---

_Comment by @zanieb on 2025-07-31 11:56_

@frbelotto open an issue with details please.

---
