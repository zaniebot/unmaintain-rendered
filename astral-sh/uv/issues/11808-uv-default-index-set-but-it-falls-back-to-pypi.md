```yaml
number: 11808
title: UV_DEFAULT_INDEX set but it falls back to pypi.org in some cases
type: issue
state: closed
author: krpatter-intc
labels:
  - bug
assignees: []
created_at: 2025-02-26T19:06:23Z
updated_at: 2025-02-26T21:12:23Z
url: https://github.com/astral-sh/uv/issues/11808
synced_at: 2026-01-12T16:00:46Z
```

# UV_DEFAULT_INDEX set but it falls back to pypi.org in some cases

---

_@krpatter-intc_

### Summary

We are setting `UV_DEFAULT_INDEX` to an internal index with credentials, but it seems at some if a failure occurs then `uv` will still fallback and try to install from pypi.org.


By "failure occurs" definition: The resolution finds an older/bad package that managed to get a bad dependency string: `bad-package>=1.1.6<2` misssing a `,` -- this is indeed an error and not currently asking for anything here without me tracking down why the dependency reconciliation tried to download the much older package. I only provide this as data for how we ended up with the error that prompted uv to reach out to `pypi.org`



### Platform

Windows 11

### Version

uv 0.6.2 (6d3614eec 2025-02-19)

### Python version

Python310

---

_Label `bug` added by @krpatter-intc on 2025-02-26 19:06_

---

_Comment by @charliermarsh on 2025-02-26 19:08_

Does your index fallback to PyPI when valid credentials are not applied? That's a somewhat common behavior and whenever this kind of report has come up in the past, it's always been due to that fallback.

---

_Comment by @krpatter-intc on 2025-02-26 19:13_

We're using a derivative of `devpi`. Devpi which still serves up the packages but as if it were a mirror. I dont think it would do a `redirect` -- when used with pip we typically just get a 401/403 type error.

---

_Comment by @krpatter-intc on 2025-02-26 19:14_

And I only see this if something else failed previously....Never does it fail with _just_ this error (with an admittedly extremely small sample size)

---

_Comment by @krpatter-intc on 2025-02-26 21:12_

I will perhaps close this and make sure I can reproduce this with file based index and re-open once I have a full reproduction close

---

_Closed by @krpatter-intc on 2025-02-26 21:12_

---
