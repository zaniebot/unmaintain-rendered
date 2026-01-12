```yaml
number: 20580
title: Remove unnecessary f-string handling in comment placement
type: pull_request
state: closed
author: dylwil3
labels:
  - internal
assignees: []
draft: true
base: main
head: remove-fstring-skip-comment-placement
created_at: 2025-09-25T18:30:25Z
updated_at: 2025-11-10T15:51:35Z
url: https://github.com/astral-sh/ruff/pull/20580
synced_at: 2026-01-12T15:57:05Z
```

# Remove unnecessary f-string handling in comment placement

---

_@dylwil3_

In #7047 some logic was added to comment placement in order to address a panic in #6898 . I believe that panic was caused by old behavior of `SimpleTokenizer` that is no longer present, because it does not occur if the new logic is simply removed.

This was discovered while trying to figure out the potential side-effects of #20578 - I originally was going to augment the logic to include t-strings here, but discovered that you could instead just remove it (unless I missed something?)


---

_Review requested from @MichaReiser by @dylwil3 on 2025-09-25 18:30_

---

_Label `internal` added by @dylwil3 on 2025-09-25 18:30_

---

_Comment by @github-actions[bot] on 2025-09-25 18:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2025-09-26 09:11_

It's very likely that this code is unused but it might be for a different reason than outlined in the PR and I'd prefer to fully understand why before removing this code.

>  I believe that panic was caused by old behavior of SimpleTokenizer that is no longer present, because it does not occur if the new logic is simply removed.

Can you explain what this new logic is? Reading https://github.com/astral-sh/ruff/issues/6898. My impression is that the issue is that the `SimpleTokenizer` doesn't support lexing strings. This hasn't changed. 

To me, it also seems that the comment is still true because the interpolated element's range only covers `{1}` but maybe that's fine? 



---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:86 on 2025-09-26 09:13_

I can't comment there but doesn't this change result in the `SimpleTokenizer` trying to lex `' # comment` on line 136. This would be invalid because the `SimpleTokenizer` should never be pointed inside a string.

---

_@MichaReiser reviewed on 2025-09-26 09:13_

---

_Converted to draft by @dylwil3 on 2025-10-20 18:38_

---

_Closed by @dylwil3 on 2025-11-10 15:51_

---
