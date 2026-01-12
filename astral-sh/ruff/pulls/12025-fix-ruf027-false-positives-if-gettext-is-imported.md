```yaml
number: 12025
title: "Fix RUF027 false positives if `gettext` is imported using an alias"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: main
head: fstring-rule-fixup
created_at: 2024-06-25T11:03:40Z
updated_at: 2024-06-25T18:10:26Z
url: https://github.com/astral-sh/ruff/pull/12025
synced_at: 2026-01-12T15:55:40Z
```

# Fix RUF027 false positives if `gettext` is imported using an alias

---

_@AlexWaygood_

Noticed this while reviewing if it was ready to be stabilised

---

_Label `rule` added by @AlexWaygood on 2024-06-25 11:04_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-25 11:04_

---

_Review requested from @snowsignal by @AlexWaygood on 2024-06-25 11:04_

---

_@AlexWaygood reviewed on 2024-06-25 11:05_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:221 on 2024-06-25 11:05_

This is a micro-optimisation opportunity I spotted. I believe `count()` will consume the whole iterator, which shouldn't be necessary for our purposes: we can short-circuit as soon as we've found 2

---

_Comment by @github-actions[bot] on 2024-06-25 11:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-06-25 11:22_

What's exactly the false positive that this PR fixes?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:106 on 2024-06-25 11:22_

```suggestion
        if id.as_str() == "_" {
            return true;
        }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:221 on 2024-06-25 11:26_

Nit: You could probably do `searcher.nth(1).is_some()`

---

_@charliermarsh approved on 2024-06-25 11:27_

---

_Comment by @charliermarsh on 2024-06-25 11:27_

I think it's demonstrated in the fixture: aliasing `gettext` when importing.

---

_@MichaReiser approved on 2024-06-25 11:27_

Nice find

---

_Comment by @charliermarsh on 2024-06-25 11:28_

The only risk here is: is `gettext` ever imported from somewhere else, like `django.gettext`? (I made that up, I have no idea.)

---

_Comment by @charliermarsh on 2024-06-25 11:28_

We might want to _also_ check the raw name.

---

_Comment by @AlexWaygood on 2024-06-25 11:29_

> I think it's demonstrated in the fixture: aliasing `gettext` when importing.

It's actually not currently; Micha made a good point -- there's a bug in my test :-)

But I'll improve the test so that it _does_ actually fail on `main`

---

_Comment by @AlexWaygood on 2024-06-25 11:37_

> The only risk here is: is `gettext` ever imported from somewhere else, like `django.gettext`? (I made that up, I have no idea.)

Ah yeah, you're right, that's apparently a thing: https://github.com/django/django/blob/d9bd58c3b8b3e8735d8242c2bb9b09c52ed6171b/django/contrib/flatpages/forms.py#L5

`gettext` is wild!

---

_Comment by @charliermarsh on 2024-06-25 11:42_

I figured there was a reason for this (Chesterton's Fence) hah.

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-06-25 11:49_

---

_@charliermarsh approved on 2024-06-25 18:00_

---

_Merged by @AlexWaygood on 2024-06-25 18:10_

---

_Closed by @AlexWaygood on 2024-06-25 18:10_

---

_Branch deleted on 2024-06-25 18:10_

---
