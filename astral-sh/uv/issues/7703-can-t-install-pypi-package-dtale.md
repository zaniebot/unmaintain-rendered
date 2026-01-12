```yaml
number: 7703
title: "Can't install pypi package dtale"
type: issue
state: closed
author: kjyv
labels:
  - error messages
assignees: []
created_at: 2024-09-26T09:44:19Z
updated_at: 2025-01-14T13:38:24Z
url: https://github.com/astral-sh/uv/issues/7703
synced_at: 2026-01-12T15:59:16Z
```

# Can't install pypi package dtale

---

_@kjyv_

Hey there,
I'm trying to install [dtale](https://github.com/man-group/dtale) with a simple pyproject.toml running `uv sync` (.venv deleted, .cache/uv deleted). Even if it is the only package, I see this error:
`error: distribution kaleido==0.2.1.post1 @ registry+https://pypi.org/simple can't be installed because it doesn't have a source distribution or wheel for the current platform`

Installing with `uv pip install dtale` work fine though and the same package combination works well with pipenv.
I'm on macOS 15.0, uv 0.4.16.

---

_Comment by @konstin on 2024-09-26 11:26_

This is caused by a difference in how `uv sync` and `uv pip install` handle unavailable wheel. The tl;dr fix for you problem is adding the following to your `pyproject.toml`:

```
[tool.uv]
constraint-dependencies = ["kaleido!=0.2.1.post1"]
```

The longer explanation: There are two recent versions of dtale, 0.2.1 and 0.2.1.post1. The former has a source distribution and wheels for all sorts of platforms (https://pypi.org/project/kaleido/0.2.1/#files), the latter only has an armv7l wheel for linux (https://pypi.org/project/kaleido/0.2.1.post1/#files); I think the authors wanted to add the missing wheel to the release, but instead created a new release. When using `uv pip install`, the resolver skips over 0.2.1.post1 because it wants to find something that works for the current platform and uses 0.2.1 instead. `uv sync` on the other hand attempts a universal resolution that is the same independent from which platform you run it, so it can't skip releases due to missing source distributions or platforms. It then locks 0.2.1.post1 and subsequently fails to install. The constraint we're adding says that if the resolver is trying to pick a version for kaleido, it should skip that post-release, but without making kaleido a dependency of your project.

---

_Label `question` added by @konstin on 2024-09-26 11:26_

---

_Comment by @kjyv on 2024-09-26 12:17_

Ok, thank you for the detailed explanation. So the contraint fixes our problem but I guess it's not ideal if users like me run into this and the error message is not saying much about how to go forward. Rather than changing the resolver for the sync which I guess is tricky for all platforms with such packages, could this case be detected by uv and a more specific error message shown?

---

_Label `question` removed by @charliermarsh on 2024-09-28 19:11_

---

_Label `error messages` added by @charliermarsh on 2024-09-28 19:11_

---

_Comment by @charliermarsh on 2024-11-20 00:37_

I think this is a case of the more general #7816.

---

_Closed by @charliermarsh on 2024-11-20 00:37_

---

_Comment by @LuchiLucs on 2025-01-14 13:33_

I'm trying to install the package `connectorx` on `py3.9` but it fails it `uv add`. What are the available options here? I also checked [7816](https://github.com/astral-sh/uv/issues/7816) and [7435](https://github.com/astral-sh/uv/issues/7435) but I would like to know if there is a recommended option

---

_Comment by @konstin on 2025-01-14 13:36_

@LuchiLucs Can you please open a new issue with a minimal reproduction of your example and information about your platform?

---

_Comment by @LuchiLucs on 2025-01-14 13:38_

@konstin Sure, sorry about this.

---
