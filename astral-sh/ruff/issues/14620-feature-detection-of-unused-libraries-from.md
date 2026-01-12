```yaml
number: 14620
title: "Feature: Detection of unused libraries from pyproject.toml"
type: issue
state: closed
author: akshaybabloo
labels:
  - packaging
assignees: []
created_at: 2024-11-27T01:49:58Z
updated_at: 2024-11-27T06:25:08Z
url: https://github.com/astral-sh/ruff/issues/14620
synced_at: 2026-01-12T15:54:54Z
```

# Feature: Detection of unused libraries from pyproject.toml

---

_@akshaybabloo_

A good to have feature would be to list out all unused libraries (dev and dependencies), so that they can be removed or used.

---

_Label `type-inference` added by @dylwil3 on 2024-11-27 02:27_

---

_Comment by @dylwil3 on 2024-11-27 02:28_

That sounds like a neat idea! It will require multi-file analysis, I think (which is included under the umbrella of "type inference").

---

_Comment by @dhruvmanila on 2024-11-27 06:24_

I think this is more of a packaging problem and it should be covered by the third point in https://github.com/astral-sh/ruff/issues/10015. Merging this issue with the linked one.

---

_Closed by @dhruvmanila on 2024-11-27 06:24_

---

_Label `type-inference` removed by @dhruvmanila on 2024-11-27 06:25_

---

_Label `packaging` added by @dhruvmanila on 2024-11-27 06:25_

---
