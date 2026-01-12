```yaml
number: 14653
title: "CI: Treat mdtest Markdown files as code"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
assignees: []
merged: true
base: main
head: david/run-ci-for-mdtest-changes
created_at: 2024-11-28T08:44:21Z
updated_at: 2024-11-28T09:06:57Z
url: https://github.com/astral-sh/ruff/pull/14653
synced_at: 2026-01-12T15:55:48Z
```

# CI: Treat mdtest Markdown files as code

---

_@sharkdp_

## Summary

Make sure we run the tests for mdtest-only changes.

## Test Plan

Tested if positive glob patterns override negative patterns here: https://codepen.io/mrmlnc/pen/OXQjMe

Will test using the changes in https://github.com/astral-sh/ruff/pull/14652 (where tests were previously skipped)

---

_Label `internal` added by @sharkdp on 2024-11-28 08:44_

---

_@AlexWaygood approved on 2024-11-28 08:47_

LGTM if it works. Is it worth pushing a temporary commit to an md file here to make sure it triggers CI, and then reverting it again afterwards?

---

_Comment by @AlexWaygood on 2024-11-28 08:49_

Oh I guess CI will always be triggered here anyway, because this PR edits the workflow file. So the temporary commit would also have to remove the line in this workflow file that means that CI is triggered if the workflow file itself is edited in a PR

---

_Comment by @github-actions[bot] on 2024-11-28 08:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @sharkdp on 2024-11-28 08:58_

I would have to be careful to remove `.github/workflows/ci.yml` everywhere and add a new `!.github/workflows/ci.yml` pattern in the code section. And then I might still miss something and we still don't know if CI ran because of the mdtest change or for some other reason.

If it's okay, I would just merge this and then rebase #14652 to test it properly?

---

_Comment by @AlexWaygood on 2024-11-28 08:59_

Sounds good!

---

_Merged by @sharkdp on 2024-11-28 09:04_

---

_Closed by @sharkdp on 2024-11-28 09:04_

---

_Branch deleted on 2024-11-28 09:04_

---

_Comment by @sharkdp on 2024-11-28 09:06_

Seems to work as intended.

---
