```yaml
number: 13249
title: Provide new unsafe-first-index strategy
type: pull_request
state: closed
author: jtfmumm
labels:
  - enhancement
assignees: []
base: main
head: jtfm/unsafe-first-index
created_at: 2025-05-01T07:45:04Z
updated_at: 2025-06-13T14:31:33Z
url: https://github.com/astral-sh/uv/pull/13249
synced_at: 2026-01-12T16:10:37Z
```

# Provide new unsafe-first-index strategy

---

_@jtfmumm_

This PR adds a new index strategy called `unsafe-first-index`. This is the same as the default `first-index` strategy except that it continues searching indexes when encountering authentication failures. This provides a way to ignore these failures without configuring an index in `pyproject.toml`, but also makes clear that it is more insecure than our default.

Usage example:
```
uv add anyio --extra-index-url private.index.url --index-strategy unsafe-first-index
``` 

I'm not certain this should be an option, but since it was straightforward to add I've created this PR for discussion. It's a little confusing that `unsafe-first-index` and the existing `unsafe-first-match` sound similar, but this is just the `first-index` strategy with ignored authentication errors.



---

_Label `enhancement` added by @jtfmumm on 2025-05-01 07:45_

---

_Closed by @jtfmumm on 2025-06-13 14:29_

---

_Comment by @jtfmumm on 2025-06-13 14:31_

If we want to make this configurable, we'd ideally use something else than an index strategy, so I'm closing for now.

---
