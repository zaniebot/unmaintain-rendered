```yaml
number: 20778
title: "[`flake8-datetimez`] Clarify docs for several rules"
type: pull_request
state: merged
author: ageorgou
labels:
  - documentation
assignees: []
merged: true
base: main
head: datetimtz-docs
created_at: 2025-10-08T22:10:18Z
updated_at: 2025-10-10T13:06:05Z
url: https://github.com/astral-sh/ruff/pull/20778
synced_at: 2026-01-10T17:34:34Z
```

# [`flake8-datetimez`] Clarify docs for several rules

---

_Pull request opened by @ageorgou on 2025-10-08 22:10_

## Summary

Resolves #19384.

- Distinguishes more clearly between `date` and `datetime` objects.
- Uniformly links to the relevant Python docs from rules in this category.

I've tried to be clearer, but there's still a contradiction in the rules as written: we say "use timezone-aware objects", but `date`s are inherently timezone-naive.

Also, the full docs don't always match the error message: for instance, in [DTZ012](https://docs.astral.sh/ruff/rules/call-date-fromtimestamp/), the example says to use:
```python
datetime.datetime.fromtimestamp(946684800, tz=datetime.UTC)
```
while `fix_title` returns "Use `datetime.datetime.fromtimestamp(ts, tz=...)**.date()**` instead".
I have left this as it was for now.

## Test Plan
Ran `mkdocs` locally and inspected result.


---

_@ntBre approved on 2025-10-09 20:25_

Thank you, this looks like a nice improvement! Would you mind fixing the CI failure? It looks like you just need to run `cargo fmt`.

I think it would also make sense to update the examples to match `fix_title`, at least for DTZ012 as you pointed out.

---

_Label `documentation` added by @ntBre on 2025-10-09 20:25_

---

_Renamed from "[flake8-datetimez] Clarify docs for some flake8-datetimez rules" to "[`flake8-datetimez`] Clarify docs for several rules" by @ntBre on 2025-10-09 20:26_

---

_Comment by @github-actions[bot] on 2025-10-09 20:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ageorgou on 2025-10-09 23:16_

Updated, let me know if that's what you had in mind.

---

_@ntBre approved on 2025-10-10 13:00_

Perfect, thanks again!

---

_Merged by @ntBre on 2025-10-10 13:02_

---

_Closed by @ntBre on 2025-10-10 13:02_

---
