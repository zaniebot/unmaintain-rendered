```yaml
number: 18542
title: "Add trailing space around `readlines`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: charlie/read
created_at: 2025-06-08T01:25:58Z
updated_at: 2025-06-08T22:25:09Z
url: https://github.com/astral-sh/ruff/pull/18542
synced_at: 2026-01-10T18:45:04Z
```

# Add trailing space around `readlines`

---

_Pull request opened by @charliermarsh on 2025-06-08 01:25_

Closes https://github.com/astral-sh/ruff/issues/17683.


---

_Label `bug` added by @charliermarsh on 2025-06-08 01:26_

---

_Label `fixes` added by @charliermarsh on 2025-06-08 01:26_

---

_Comment by @github-actions[bot] on 2025-06-08 01:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2025-06-08 16:00_

---

_Closed by @charliermarsh on 2025-06-08 16:00_

---

_Branch deleted on 2025-06-08 16:00_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/refurb/FURB129.py`:109 on 2025-06-08 20:14_

Sorry for the late review, but these three cases don't actually trigger FURB129, so I'm not sure if they're necessary. I noticed this while rebasing the 0.12 branch.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/readlines_in_for.rs`:109 on 2025-06-08 20:17_

While I'm here, I also think this section would be a bit nicer as:

```rust
    let deletion_range = if let Some(parenthesized_range) = parenthesized_range(
        expr_attr.value.as_ref().into(),
        expr_attr.into(),
        checker.comment_ranges(),
        checker.source(),
    ) {
        expr_call.range().add_start(parenthesized_range.len())
    } else {
        expr_call.range().add_start(expr_attr.value.range().len())
    };

    let padded = pad_end(String::new(), deletion_range.end(), checker.locator());
    let edit = if padded.is_empty() {
        Edit::range_deletion(deletion_range)
    } else {
        Edit::range_replacement(padded, deletion_range)
    };
```

---

_@ntBre reviewed on 2025-06-08 20:18_

---

_@charliermarsh reviewed on 2025-06-08 22:18_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/refurb/rules/readlines_in_for.rs`:109 on 2025-06-08 22:18_

Will change!

---

_@charliermarsh reviewed on 2025-06-08 22:19_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/refurb/FURB129.py`:109 on 2025-06-08 22:19_

I think this is sort of marginal -- I see some value / little cost in having these kinds of pathological cases here just to make sure they don't cause a panic, but taking that attitude to the extreme is obviously untenable :) I can remove since I'll be doing a follow-up PR anyway, but if I weren't changing the other piece in `readlines_in_for.rs`, I'd probably opt to just leave these here.

---

_@ntBre reviewed on 2025-06-08 22:25_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/refurb/FURB129.py`:109 on 2025-06-08 22:25_

Makes sense, I can see the value too! I was just afraid I dropped a few diagnostics while rebasing until I realized they weren't meant to trigger. I'm happy either way in both cases here.

---
