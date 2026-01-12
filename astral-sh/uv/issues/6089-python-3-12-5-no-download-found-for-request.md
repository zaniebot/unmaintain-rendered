```yaml
number: 6089
title: "Python 3.12.5 \"No download found for request\""
type: issue
state: closed
author: pdpark
labels: []
assignees: []
created_at: 2024-08-14T16:40:39Z
updated_at: 2024-08-14T16:41:23Z
url: https://github.com/astral-sh/uv/issues/6089
synced_at: 2026-01-12T15:59:01Z
```

# Python 3.12.5 "No download found for request"

---

_@pdpark_

I can see 3.12.5 builds in the latest [release](https://github.com/indygreg/python-build-standalone/releases/tag/20240814) but I am unable to install it:

```shell
uv python install --native-tls 3.12.5
warning: `uv python install` is experimental and may change without warning
Searching for Python versions matching: Python 3.12.5
error: No download found for request: cpython-3.12.5-macos-aarch64-none
```


---

_Comment by @charliermarsh on 2024-08-14 16:41_

We bundle these with `uv`. They'll be available as of the next `uv` release.

---

_Closed by @charliermarsh on 2024-08-14 16:41_

---

_Comment by @charliermarsh on 2024-08-14 16:41_

(Bundle the URLs, that is.)

---
