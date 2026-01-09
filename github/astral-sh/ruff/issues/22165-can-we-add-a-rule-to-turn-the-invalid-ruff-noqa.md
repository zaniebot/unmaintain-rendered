---
number: 22165
title: "Can we add a rule to turn the `Invalid # ruff: noqa directive` warnings into an error?"
type: issue
state: closed
author: yilei
labels: []
assignees: []
created_at: 2025-12-23T19:04:59Z
updated_at: 2025-12-23T20:56:49Z
url: https://github.com/astral-sh/ruff/issues/22165
synced_at: 2026-01-07T13:12:16-06:00
---

# Can we add a rule to turn the `Invalid # ruff: noqa directive` warnings into an error?

---

_Issue opened by @yilei on 2025-12-23 19:04_

### Summary

When ruff sees noqa comments with the wrong format, it emits warnings like:

```
warning: Invalid `# ruff: noqa` directive at path/to/file:1: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
```

We use ruff with pre-commit. The issue we often run into is when `pre-commit` fails with real errors / autofixes, users are confused with these unrelated warnings and thinking these are the issues they need to fix but they are unrelated to their change. So we want to either have a way to silence these warnings in the output (that's https://github.com/astral-sh/ruff/issues/9792), or enable a rule to not ever allow code with invalid noqa comments merged. This FR is for the latter.

---

_Comment by @ntBre on 2025-12-23 20:56_

Thanks for the report! I believe this is a duplicate of #8157. I agree with you and the comments there that this would be great to have, it's just a bit tricky to implement currently.

---

_Closed by @ntBre on 2025-12-23 20:56_

---
