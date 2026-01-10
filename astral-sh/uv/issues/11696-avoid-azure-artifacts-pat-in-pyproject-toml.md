```yaml
number: 11696
title: avoid azure artifacts PAT in pyproject.toml
type: issue
state: closed
author: zkurtz
labels:
  - question
assignees: []
created_at: 2025-02-21T12:11:36Z
updated_at: 2025-03-09T12:22:42Z
url: https://github.com/astral-sh/uv/issues/11696
synced_at: 2026-01-10T03:50:31Z
```

# avoid azure artifacts PAT in pyproject.toml

---

_Issue opened by @zkurtz on 2025-02-21 12:11_

### Question

I defined `UV_INDEX` following [this documentation](https://docs.astral.sh/uv/guides/integration/alternative-indexes/#using-a-pat).

But then adding any pypi package like, say, `uv add pandas` ends up adding 
```
[[tool.uv.index]]
url = ...
```
to my pyproject.toml, where the url includes my PAT. I would rather not have my PAT git-tracked. What's the recommended solution?

### Platform

Darwin 24.3.0 arm64

### Version

uv 0.5.4

---

_Label `question` added by @zkurtz on 2025-02-21 12:11_

---

_Comment by @FishAlchemist on 2025-02-22 04:02_

Since you're already using `[[tool.uv.index]]` in your `pyproject.toml`, perhaps you could also try this? However, I'm not sure from which version this feature is supported.
https://docs.astral.sh/uv/configuration/indexes/#providing-credentials

---

_Comment by @zkurtz on 2025-02-22 14:27_

That worked! Here is a PR to clarify the docs https://github.com/astral-sh/uv/pull/11709

---

_Comment by @zkurtz on 2025-02-26 15:49_

TODO: review whether any part of my PR linked above is still relevant/needed after https://github.com/astral-sh/uv/pull/10826 lands

---

_Closed by @zkurtz on 2025-03-09 12:22_

---
