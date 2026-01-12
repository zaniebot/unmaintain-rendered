```yaml
number: 4612
title: "Remove `ReferenceContext::Synthetic`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/synthetic
created_at: 2023-05-23T20:16:48Z
updated_at: 2023-05-24T14:57:49Z
url: https://github.com/astral-sh/ruff/pull/4612
synced_at: 2026-01-12T15:55:16Z
```

# Remove `ReferenceContext::Synthetic`

---

_@charliermarsh_

## Summary

This is a confusing concept -- we're adding bindings that refer to `TextRange::default()`, so you know it's not great. Looking through the areas where we rely on `Binding::is_used`, most of them gate on `BindingKind` anyway, so I'm not sure that this concept is needed in as many places as I'd thought. Instead, I'd rather push logic to individual checks to ensure that they're operating on the correct bindings.


---

_Comment by @charliermarsh on 2023-05-23 20:17_

(I need to see whether the ecosystem CI picks up anything here, and perhaps add some new test cases.)

---

_Comment by @github-actions[bot] on 2023-05-23 20:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-05-24 07:12_

---

_Merged by @charliermarsh on 2023-05-24 14:30_

---

_Closed by @charliermarsh on 2023-05-24 14:30_

---

_Branch deleted on 2023-05-24 14:30_

---
