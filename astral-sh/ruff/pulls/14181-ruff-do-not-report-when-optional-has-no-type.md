```yaml
number: 14181
title: "[`ruff`] Do not report when `Optional` has no type arguments (`RUF013`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: RUF013
created_at: 2024-11-08T00:05:31Z
updated_at: 2024-11-10T04:36:55Z
url: https://github.com/astral-sh/ruff/pull/14181
synced_at: 2026-01-12T15:55:47Z
```

# [`ruff`] Do not report when `Optional` has no type arguments (`RUF013`)

---

_@InSyncWithFoo_

## Summary

Resolves #13833.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-08 00:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @MichaReiser on 2024-11-08 08:12_

---

_@MichaReiser requested changes on 2024-11-08 08:14_

Thank you for looking into this issue.

The check looks good to me, but I think we should move it into `type_hint_explicitly_allows_none` so that `Annotated[Optional, "bla"]` also works

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-11-08 13:48_

---

_@charliermarsh approved on 2024-11-09 02:47_

---

_Label `rule` removed by @charliermarsh on 2024-11-09 02:47_

---

_Label `bug` added by @charliermarsh on 2024-11-09 02:47_

---

_Merged by @charliermarsh on 2024-11-09 13:48_

---

_Closed by @charliermarsh on 2024-11-09 13:48_

---

_Comment by @henryiii on 2024-11-09 14:05_

I'm confused, bare `Optional` still isn't a valid type annotation for type checkers, isn't it? `Optional` would be `Optional[Any]`, which expands to `Union[None, Any]`, but a `Union` with `Any` is just `Any`. I thought the problem was the suggested fix, `Optional[Optional]` was nonsense. `Optional[Any]` would make more sense and I believe type checkers are okay with that (even if it's no different than just `Any`).

---

_Comment by @charliermarsh on 2024-11-09 15:03_

That’s correct but seems outside the scope of the rule? Like a bare Optional being invalid should be its own rule.

---

_Comment by @henryiii on 2024-11-09 15:05_

Ah, yes, that makes sense.

---

_Branch deleted on 2024-11-10 04:36_

---
