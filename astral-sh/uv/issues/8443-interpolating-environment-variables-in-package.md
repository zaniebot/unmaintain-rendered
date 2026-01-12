```yaml
number: 8443
title: Interpolating environment variables in Package Indexes
type: issue
state: closed
author: arnaldojvg
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-10-22T10:31:16Z
updated_at: 2024-10-22T11:21:36Z
url: https://github.com/astral-sh/uv/issues/8443
synced_at: 2026-01-12T15:59:26Z
```

# Interpolating environment variables in Package Indexes

---

_@arnaldojvg_

Hello! 

First of all I want to say thanks for this great tool :)

I'm currently in an effort to move away from Pipfile/pipenv to use `uv`, but I'm facing a challenge:

I can't define an [index](https://docs.astral.sh/uv/configuration/indexes/) that has credentials as a proper index in my pyproject.toml

```
[[tool.uv.index]]
name = "pypi-custom"
url = "https://aws:$CODEARTIFACT_AUTH_TOKEN@host.d.codeartifact.eu-central-1.amazonaws.com/pypi/python/simple/"
```

As it always gives me a 401.

I managed to make it work using the [Alternative Indexes](https://docs.astral.sh/uv/guides/integration/alternative-indexes/#aws-codeartifact) approach + some workarounds.

However, this doesn't allow me to specify from which index to install which package, and entirely rely on the resolution strategy (fallback), which, in my current project is not an issue, but it is definitely a weak point.

Is there any way to make `uv` interpolate the environment variables in the .toml files?

---

_Comment by @zanieb on 2024-10-22 10:55_

Hi! Glad you like the tool. There's not a way to do this right now, you can track support in https://github.com/astral-sh/uv/issues/5734

Instead, I'd recommend setting `UV_INDEX_PYPI_CUSTOM_PASSWORD="$CODEARTIFACT_AUTH_TOKEN"` — we'll use it as the password at runtime then.

---

_Label `duplicate` added by @zanieb on 2024-10-22 10:55_

---

_Label `question` added by @zanieb on 2024-10-22 10:55_

---

_Closed by @arnaldojvg on 2024-10-22 11:04_

---

_Comment by @arnaldojvg on 2024-10-22 11:21_

Thanks @zanieb will watch the other issue to be aware, this should work!
Appreciate the quick answer.

---
