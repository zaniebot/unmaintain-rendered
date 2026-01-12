```yaml
number: 20227
title: "[`refurb`] Fix safety section (`FURB132`)"
type: pull_request
state: closed
author: VascoSch92
labels:
  - documentation
assignees: []
base: main
head: fix-safety-check-and-remove-from-set
created_at: 2025-09-04T12:14:36Z
updated_at: 2025-09-09T14:59:37Z
url: https://github.com/astral-sh/ruff/pull/20227
synced_at: 2026-01-12T15:56:57Z
```

# [`refurb`] Fix safety section (`FURB132`)

---

_@VascoSch92_

The PR add the fix safety section for rule `FURB132` (#15584 )

---

_Comment by @github-actions[bot] on 2025-09-04 12:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-09-04 20:25_

Thank you!

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/check_and_remove_from_set.rs`:43 on 2025-09-04 20:26_

Hmm... but won't the `if` check make that impossible?

---

_@dylwil3 requested changes on 2025-09-04 20:26_

On second thought - it's not clear to me that the justification makes sense here. Could you provide an example where the fix actually does this?

---

_Label `documentation` added by @ntBre on 2025-09-04 21:39_

---

_@VascoSch92 reviewed on 2025-09-07 19:21_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/refurb/rules/check_and_remove_from_set.rs`:43 on 2025-09-07 19:21_

Actually, you are right. I checked and I could not find a situation where we have a change of behaviour in the program. I think the only problem is that it can delete comments

---

_Comment by @dylwil3 on 2025-09-09 12:48_

If you're up for it I think it would make sense to open a different PR where (gated under preview) you make the fix safe if the edit range does not intersect comments, and unsafe otherwise.

What happened here and elsewhere is that long long ago the fix was implemented as "suggested". Later all "suggested" fixes were switched to "sometimes applies", and later still all "sometimes applies" fixes were switched to "unsafe". There's quite a big semantic gap between "suggested" and "unsafe", and I think that's why a lot of the rules labeled as unsafe have left us scratching our heads.

---

_Closed by @dylwil3 on 2025-09-09 12:48_

---

_Comment by @VascoSch92 on 2025-09-09 14:59_

> If you're up for it I think it would make sense to open a different PR where (gated under preview) you make the fix safe if the edit range does not intersect comments, and unsafe otherwise.
> 
> What happened here and elsewhere is that long long ago the fix was implemented as "suggested". Later all "suggested" fixes were switched to "sometimes applies", and later still all "sometimes applies" fixes were switched to "unsafe". There's quite a big semantic gap between "suggested" and "unsafe", and I think that's why a lot of the rules labeled as unsafe have left us scratching our heads.

make sense... I will give a look :-)

---
