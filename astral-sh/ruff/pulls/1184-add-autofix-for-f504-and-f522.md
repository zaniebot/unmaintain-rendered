```yaml
number: 1184
title: Add autofix for F504 and F522
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: 1144-autofix-f504-f522
created_at: 2022-12-10T19:02:14Z
updated_at: 2022-12-27T05:42:56Z
url: https://github.com/astral-sh/ruff/pull/1184
synced_at: 2026-01-12T15:55:05Z
```

# Add autofix for F504 and F522

---

_@squiddy_

Refs: #1144

---

_@charliermarsh reviewed on 2022-12-10 19:56_

---

_Review comment by @charliermarsh on `src/pyflakes/fixes.rs`:108 on 2022-12-10 19:56_

What case is this handling?

---

_Review comment by @squiddy on `src/pyflakes/fixes.rs`:108 on 2022-12-10 20:15_

This one is a bit weird, perhaps there is a better way. LibCST represents dictionary keys as SimpleStrings (at least in my tests so far), so a key like `foo` is `"foo"`.

Thinking about it, this would need handling of `r`/`b` prefix as well.

---

_@squiddy reviewed on 2022-12-10 20:15_

---

_Converted to draft by @squiddy on 2022-12-10 20:18_

---

_@charliermarsh reviewed on 2022-12-10 20:24_

---

_Review comment by @charliermarsh on `src/pyflakes/fixes.rs`:108 on 2022-12-10 20:24_

You might be able to use `TRIPLE_QUOTE_PREFIXES` or `SINGLE_QUOTE_PREFIXES` from `src/docstrings/constants.rs`...?

---

_@squiddy reviewed on 2022-12-10 21:15_

---

_Review comment by @squiddy on `src/pyflakes/fixes.rs`:108 on 2022-12-10 21:15_

I've added a `strip_quotes_and_prefixes` instead, because `SINGLE_QUOTE_PREFIXES` doesn't handle byte strings.

---

_Marked ready for review by @squiddy on 2022-12-10 21:19_

---

_Merged by @charliermarsh on 2022-12-10 21:33_

---

_Closed by @charliermarsh on 2022-12-10 21:33_

---

_Comment by @charliermarsh on 2022-12-10 21:46_

Thanks for this, awesome work.

---

_Branch deleted on 2022-12-27 05:42_

---
