```yaml
number: 17802
title: "[`pylint`] add fix safety section (`PLC0414`)"
type: pull_request
state: merged
author: yunchipang
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/fix-safety-useless-import-alias
created_at: 2025-05-02T22:41:51Z
updated_at: 2025-05-11T16:01:27Z
url: https://github.com/astral-sh/ruff/pull/17802
synced_at: 2026-01-10T18:57:03Z
```

# [`pylint`] add fix safety section (`PLC0414`)

---

_Pull request opened by @yunchipang on 2025-05-02 22:41_

This PR adds a fix safety section in comment for rule `PLC0414`.

parent: #15584 
discussion: #6294

---

_Comment by @yunchipang on 2025-05-03 02:57_

@dylwil3 please let me know if this PR summary is what you were looking for (mentioned in https://github.com/astral-sh/ruff/issues/15584#issue-2797743633). thanks!

---

_Label `documentation` added by @AlexWaygood on 2025-05-03 19:37_

---

_Comment by @github-actions[bot] on 2025-05-03 19:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dylwil3 on 2025-05-06 14:08_

Hi @yunchipang - thanks for looking into this! This is a pretty old rule that has survived many refactors of the code base, and I think the PR you're linking to isn't quite the one that originated the unsafe fix. So I don't think the reason you're giving for fix unsafety is correct - especially because, as you point out, Ruff will detect that situation and warn the user.

I think some hints for the unsafety are in this discussion: https://github.com/astral-sh/ruff/issues/6294

Something like:

```
This fix is marked as always unsafe because the user may be intentionally re-exporting the import.
```

---

_Comment by @yunchipang on 2025-05-06 23:20_

@dylwil3 thanks so much for the input! I've looked into the issue and updated the docs, please let me know if it's clearer this time.

---

_@dylwil3 approved on 2025-05-11 16:01_

Perfect, thank you!

---

_Merged by @dylwil3 on 2025-05-11 16:01_

---

_Closed by @dylwil3 on 2025-05-11 16:01_

---
