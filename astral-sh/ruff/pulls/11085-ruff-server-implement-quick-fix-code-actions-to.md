```yaml
number: 11085
title: "`ruff server`: Implement quick fix code actions to ignore a diagnostic with `# noqa:`"
type: pull_request
state: closed
author: snowsignal
labels:
  - server
assignees: []
base: main
head: jane/server/noqa-comments
created_at: 2024-04-22T01:14:03Z
updated_at: 2025-02-20T09:00:11Z
url: https://github.com/astral-sh/ruff/pull/11085
synced_at: 2026-01-12T15:55:34Z
```

# `ruff server`: Implement quick fix code actions to ignore a diagnostic with `# noqa:`

---

_@snowsignal_

## Summary

Fixes #10594.

Note: this PR is in draft because the actual code to generate `noqa` comment edits has yet to be implemented. So far, the existing code just sets things up to accept these `noqa` comment edits.

## Test Plan

Will be created after this PR gets taken out of draft.

---

_Label `server` added by @snowsignal on 2024-04-22 01:14_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-04-22 01:14_

---

_Comment by @github-actions[bot] on 2024-04-22 03:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @codspeed-hq[bot] on 2024-04-28 03:32_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jane/server/noqa-comments)

### Merging #11085 will **not alter performance**

<sub>Comparing <code>jane/server/noqa-comments</code> (0c40149) with <code>main</code> (349a4cf)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Marked ready for review by @snowsignal on 2024-05-03 02:19_

---

_@dhruvmanila reviewed on 2024-05-03 05:04_

I think it would be useful to have two separate PRs or commits (whichever you prefer) - one which updates the `add_noqa` API on the linter side and the other which uses it in the server for the code action. This would be helpful in understanding the motivation behind the change and also allow us to look at them in isolation. I could even check if I can plug the new API for Jupyter Notebook :)

---

_@MichaReiser requested changes on 2024-05-03 06:20_

Would you mind expanding your PR summary with some more detail about what the change is about and add a test plan for both ruff CLI and the vs code extenion.

---

_Comment by @snowsignal on 2024-05-03 13:49_

> I think it would be useful to have two separate PRs or commits (whichever you prefer) - one which updates the add_noqa API on the linter side and the other which uses it in the server for the code action. This would be helpful in understanding the motivation behind the change and also allow us to look at them in isolation. I could even check if I can plug the new API for Jupyter Notebook :)

Good idea, I'll split this PR into two separate PRs.

---

_Closed by @snowsignal on 2024-05-03 13:49_

---

_Branch deleted on 2025-02-20 09:00_

---
