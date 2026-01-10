```yaml
number: 15706
title: "[`flynt`] Do not offer a fix when the new string is too long (`FLY002`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - fixes
assignees: []
base: main
head: FLY002
created_at: 2025-01-24T03:13:53Z
updated_at: 2025-02-02T19:45:19Z
url: https://github.com/astral-sh/ruff/pull/15706
synced_at: 2026-01-10T19:57:22Z
```

# [`flynt`] Do not offer a fix when the new string is too long (`FLY002`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-24 03:13_

## Summary

Resolves #5150.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @InSyncWithFoo on 2025-01-24 03:14_

This is just a workaround rather than a proper fix, since I'm not sure how to make the generator generate multiline, dedented strings.

---

_Comment by @github-actions[bot] on 2025-01-24 03:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @MichaReiser on 2025-01-24 10:01_

---

_Comment by @MichaReiser on 2025-01-24 10:02_

Hm I think we actually stopped the practice of not offering a fix if the resulting expression becomes too long. The only exception to this are some of the simplify rules because the expression length is considered an approximation for "simplyfing".

So I think we should simply close the issue instead and hint users towards suppressing the rule in this particular case.

But I must admit I can see some argument for respecting the line length, at least when targeting pre Python 3.12. 

---

_Closed by @MichaReiser on 2025-02-02 19:45_

---
