---
number: 13557
title: "Allow specifying a Python version range in `no-binary-package`"
type: issue
state: open
author: millerdev
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2025-05-20T15:34:08Z
updated_at: 2025-05-20T20:12:31Z
url: https://github.com/astral-sh/uv/issues/13557
synced_at: 2026-01-10T01:25:35Z
---

# Allow specifying a Python version range in `no-binary-package`

---

_Issue opened by @millerdev on 2025-05-20 15:34_

### Summary

Consider a project that currently uses Python 3.9, and is beginning the process of upgrading to Python 3.13.

The project requires both `lxml==5.3.0` and `xmlsec==1.3.14`, both of which install fine on Python 3.9, but are [broken on Python 3.13](https://github.com/xmlsec/python-xmlsec/issues/350). Newer versions (5.4.0 and 1.3.15, respectively) also have the [same issue](https://github.com/xmlsec/python-xmlsec/issues/344).

Similar to how it is possible to add `--no-binary lxml` to a *requirements.txt* file, it is possible to encode that in *pyproject.toml* as:

```toml
[tool.uv]
no-binary-package = ["lxml"]
```

Requesting a feature to add a `no-binary-package` directive in *pyproject.toml* that applies to (for example) Python 3.13, but not Python 3.9.

### Example

As [suggested on Discord](https://discord.com/channels/1039017663004942429/1374395648438435881/1374406369142374581), the syntax could be something like:

```toml
no-binary-package = ["lxml; python_version >= '3.13'"]
```
or
```toml
no-binary-packaage = [{package = "lxml", python_version = ">=3.13"}]
```

---

_Label `enhancement` added by @millerdev on 2025-05-20 15:34_

---

_Renamed from "uv no-binary-package with python version specifier" to "no-binary-package with python version specifier" by @millerdev on 2025-05-20 15:35_

---

_Renamed from "no-binary-package with python version specifier" to "Allow specifying a Python version range in `no-binary-package`" by @zanieb on 2025-05-20 15:35_

---

_Label `configuration` added by @zanieb on 2025-05-20 15:35_

---

_Comment by @zanieb on 2025-05-20 15:36_

This seems reasonable to me and probably pretty easy? Especially if we do the latter suggestion which does not allow arbitrary markers. @konstin thoughts?

---

_Comment by @konstin on 2025-05-20 17:51_

To clarify (I had trouble reading this out of the linked issues), this means that there are `xmlsec-1.3.15-cp313-cp313-*.whl` wheels, but while the cp39 to cp312 wheels work, the cp313 one works?

I'm not against this per-se, but I think it's too rare to justify a feature in uv, when the alternative is pinning a lower version.

---

_Comment by @millerdev on 2025-05-20 19:39_

@konstin To my knowledge there no version of `xmlsec` that works (on Linux) with the wheels published for `lxml>=5.3.0` because `xmlsec` has not published `manylinux` wheels for Python 3.13.

> this means that there are `xmlsec-1.3.15-cp313-cp313-*.whl` wheels

Only for Windows and Mac, not Linux (see [files on pypi](https://pypi.org/project/xmlsec/1.3.15/#files)).

Are you suggesting that we downgrade to an earlier version of `lxml` that does not publish wheels for Python 3.13 to work around the issue?

---

_Comment by @konstin on 2025-05-20 20:12_

Until upstream yanks the releases and publishes a fixed version, yes, this is also what airflow does: https://github.com/xmlsec/python-xmlsec/issues/344#issuecomment-2716781066

---
