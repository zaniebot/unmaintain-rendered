```yaml
number: 17932
title: "[`pylint`] add fix safety section (`PLW1514`)"
type: pull_request
state: merged
author: yunchipang
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/fix-safety-unspecified-encoding
created_at: 2025-05-08T03:59:09Z
updated_at: 2025-05-11T17:25:13Z
url: https://github.com/astral-sh/ruff/pull/17932
synced_at: 2026-01-10T18:57:03Z
```

# [`pylint`] add fix safety section (`PLW1514`)

---

_Pull request opened by @yunchipang on 2025-05-08 03:59_

parent #15584 
fix was made unsafe at #8928

---

_Comment by @yunchipang on 2025-05-08 04:03_

@ntBre @dscorbett @dylwil3 not entirely confident about the example i provided. would appreciate your inputs!

---

_Comment by @github-actions[bot] on 2025-05-08 04:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:31 on 2025-05-09 20:35_

```suggestion
/// ## Fix safety
```

I think we usually put this section last too.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:40 on 2025-05-09 20:43_

This is pretty good, and I like the example. I'm just looking for a way to say something like "this fix always adds `encoding="utf-8"` even if that's not the default on the current platform." I think that's the bigger issue than the file's actual encoding. Adding `encoding=locale.getpreferredencoding(False)` would preserve the program's behavior, but we assume that people meant UTF-8.

---

_@ntBre requested changes on 2025-05-09 20:43_

---

_Review requested from @ntBre by @yunchipang on 2025-05-09 21:42_

---

_@dylwil3 approved on 2025-05-11 16:02_

LGTM, thank you!

---

_Comment by @dylwil3 on 2025-05-11 16:04_

hm - I guess it won't let me merge until @ntBre approves because there were requested changes

---

_@ntBre approved on 2025-05-11 17:16_

---

_Comment by @ntBre on 2025-05-11 17:16_

Oops, sorry for blocking. Thank you both!

---

_Merged by @dylwil3 on 2025-05-11 17:25_

---

_Closed by @dylwil3 on 2025-05-11 17:25_

---

_Label `documentation` added by @dylwil3 on 2025-05-11 17:25_

---
