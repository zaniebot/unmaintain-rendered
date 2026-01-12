```yaml
number: 10127
title: "docs: fix invalid syntax in some sources examples"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/fix-invalid-syntax-in-sources-examples
created_at: 2024-12-23T20:34:37Z
updated_at: 2024-12-24T01:29:22Z
url: https://github.com/astral-sh/uv/pull/10127
synced_at: 2026-01-12T16:09:08Z
```

# docs: fix invalid syntax in some sources examples

---

_@mkniewallner_

## Summary

TOML 1.0 doesn't support multi-line for inline tables, so those examples are invalid.

---

_@charliermarsh approved on 2024-12-23 21:12_

Thank you!

---

_Label `documentation` added by @charliermarsh on 2024-12-23 21:12_

---

_Merged by @charliermarsh on 2024-12-23 21:12_

---

_Closed by @charliermarsh on 2024-12-23 21:12_

---

_Branch deleted on 2024-12-23 21:14_

---

_Comment by @zanieb on 2024-12-24 00:54_

Oh this is tragic — these were intentionally split for readability otherwise you have to scroll to see the full example. Is there anything we can do about that?

---

_Comment by @mkniewallner on 2024-12-24 01:11_

> Oh this is tragic — these were intentionally split for readability otherwise you have to scroll to see the full example. Is there anything we can do about that?

There's another way yes. Taking the first example, it could also be written as:

```toml
[tool.uv.sources.httpx]
git = "https://github.com/encode/httpx"
tag = "0.27.0"
```

Happy to do a follow-up PR if you think this would be better.

---

_Comment by @zanieb on 2024-12-24 01:29_

I'm not sure, to be honest. @charliermarsh do you have thoughts?

---
