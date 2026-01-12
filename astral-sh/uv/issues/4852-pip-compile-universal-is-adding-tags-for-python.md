```yaml
number: 4852
title: pip compile --universal is adding tags for python versions lower than requires-python
type: issue
state: closed
author: inoa-jboliveira
labels:
  - bug
  - lock
assignees: []
created_at: 2024-07-07T02:42:11Z
updated_at: 2024-07-09T17:58:02Z
url: https://github.com/astral-sh/uv/issues/4852
synced_at: 2026-01-12T15:58:52Z
```

# pip compile --universal is adding tags for python versions lower than requires-python

---

_@inoa-jboliveira_

Hi everyone, thank you for adding the new `--universal` option to pip compile, it will simplify a lot my workflow.

One thing I believe is a bit weird is that it adds marks for python versions below the ones on `pyproject.toml` for the `requires-python` attribute.

Currently we have `requires-python = '>=3.9'` but I see several lines with markers like: 

```
tornado==6.4 ; python_version >= '3.5.2'
python3-openid==3.2.0 ; python_version >= '3.0'
```

I really don't know when it decides the marker is mandatory, but IMO it should be only for the supported versions of my app.

Thank you


<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @inoa-jboliveira on 2024-07-07 02:56_

I forgot to say, the reason it is important to me not having these marks is because we have a few legacy envs where we need to package all the new dependencies with our binaries and these lines make it appear as if the dependency has changed.

I will do a workaround for now, but would appreciate if the output was consistent with only the supported python versions

---

_Label `bug` added by @zanieb on 2024-07-07 16:56_

---

_Label `lock` added by @zanieb on 2024-07-07 16:56_

---

_Comment by @charliermarsh on 2024-07-07 19:02_

Somewhat torn on whether we should be emitting these... not strongly opposed to stripping them though.

---

_Comment by @charliermarsh on 2024-07-08 21:17_

I have a fix for this, will push when I land.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-08 21:18_

---

_Closed by @charliermarsh on 2024-07-09 16:15_

---

_Comment by @inoa-jboliveira on 2024-07-09 17:58_

Thank you! I will try out once released

---
