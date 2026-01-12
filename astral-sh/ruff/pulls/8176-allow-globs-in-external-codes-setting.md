```yaml
number: 8176
title: "Allow globs in `external` codes setting"
type: pull_request
state: closed
author: zanieb
labels:
  - configuration
assignees: []
draft: true
base: main
head: zanie/external-glob
created_at: 2023-10-24T17:22:20Z
updated_at: 2023-10-24T18:04:02Z
url: https://github.com/astral-sh/ruff/pull/8176
synced_at: 2026-01-12T15:55:25Z
```

# Allow globs in `external` codes setting

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/8174

---

_Label `configuration` added by @zanieb on 2023-10-24 17:22_

---

_Comment by @zanieb on 2023-10-24 17:23_

Alternatively, we could just match prefixes i.e. _assume_ everything ends in `*`. This would be easier to use and might make more sense than globs for this purpose, however, it is technically a breaking change.

---

_Comment by @github-actions[bot] on 2023-10-24 17:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-10-24 17:55_

I think I'm partial to having the same logic we use for selectors elsewhere -- so, e.g., `DOC` matches `DOC123`, `DOC1`, etc. (Prefixes rather than globs, per your comment.) In what sense is it a breaking change? It feels strictly more lenient, so it wouldn't introduce any new violations.

---

_Comment by @zanieb on 2023-10-24 17:57_

I think I prefer prefixes too. I wish I hadn't bothered implementing it this way first haha

> In what sense is it a breaking change? It feels strictly more lenient, so it wouldn't introduce any new violations.

Well we'll "no longer detect" unused noqa statements that would previously be have been raised, but no new violations is true. I think it's a breaking change in the most literal of senses, not something I'm worried about.

---

_Closed by @zanieb on 2023-10-24 18:04_

---
