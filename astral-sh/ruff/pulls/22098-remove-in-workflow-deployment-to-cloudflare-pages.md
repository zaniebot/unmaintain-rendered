```yaml
number: 22098
title: Remove in-workflow deployment to Cloudflare Pages
type: pull_request
state: merged
author: woodruffw
labels:
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ww/dewrangle
created_at: 2025-12-19T21:51:24Z
updated_at: 2025-12-20T15:21:49Z
url: https://github.com/astral-sh/ruff/pull/22098
synced_at: 2026-01-10T16:36:18Z
```

# Remove in-workflow deployment to Cloudflare Pages

---

_Pull request opened by @woodruffw on 2025-12-19 21:51_

## Summary

astral-bot now does these Cloudflare Pages deploys, so that we can do them even on unprivileged PRs.

Once this is merged, we can remove the Cloudflare secrets from CI (and revoke them).

~~WIP, needs testing. I'm going to deploy a change to the bot in the moment that will hopefully remove the need for this.~~

## Test Plan

Mucking around.

---

_Assigned to @woodruffw by @woodruffw on 2025-12-19 21:51_

---

_Label `ecosystem-analyzer` added by @woodruffw on 2025-12-19 21:54_

---

_Label `ecosystem-analyzer` removed by @woodruffw on 2025-12-19 22:04_

---

_Label `ecosystem-analyzer` added by @woodruffw on 2025-12-19 22:04_

---

_Label `ecosystem-analyzer` removed by @woodruffw on 2025-12-19 22:17_

---

_Label `ecosystem-analyzer` added by @woodruffw on 2025-12-19 22:18_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 22:23_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 0 | 1 | 8 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-argument-type` | 0 | 0 | 4 |
| **Total** | **0** | **1** | **17** |


**[Full report with detailed diff](https://bfd6e7c8.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://bfd6e7c8.ty-ecosystem-ext.pages.dev/timing))



---

_Label `ecosystem-analyzer` removed by @woodruffw on 2025-12-19 22:27_

---

_Label `ecosystem-analyzer` added by @woodruffw on 2025-12-19 22:27_

---

_Label `ecosystem-analyzer` removed by @woodruffw on 2025-12-19 22:43_

---

_Label `ecosystem-analyzer` added by @woodruffw on 2025-12-19 22:43_

---

_Label `ecosystem-analyzer` removed by @woodruffw on 2025-12-19 23:00_

---

_Label `ecosystem-analyzer` added by @woodruffw on 2025-12-19 23:00_

---

_Label `ecosystem-analyzer` removed by @woodruffw on 2025-12-19 23:26_

---

_Label `ecosystem-analyzer` added by @woodruffw on 2025-12-19 23:26_

---

_Marked ready for review by @woodruffw on 2025-12-19 23:33_

---

_Review requested from @sharkdp by @woodruffw on 2025-12-19 23:33_

---

_Review requested from @AlexWaygood by @woodruffw on 2025-12-19 23:33_

---

_Comment by @woodruffw on 2025-12-19 23:34_

This now works 🙂 -- the render is the same as before, but the ecosystem page is actually being deployed by the bot instead of by CI.

---

_@sharkdp approved on 2025-12-20 08:28_

Thank you so much!

---

_Merged by @woodruffw on 2025-12-20 15:21_

---

_Closed by @woodruffw on 2025-12-20 15:21_

---

_Branch deleted on 2025-12-20 15:21_

---
