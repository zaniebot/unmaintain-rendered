```yaml
number: 17170
title: "[red-knot] Fix more [redundant-cast] false positives"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/cast-todos
created_at: 2025-04-03T11:34:25Z
updated_at: 2025-04-03T14:00:02Z
url: https://github.com/astral-sh/ruff/pull/17170
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Fix more [redundant-cast] false positives

---

_@AlexWaygood_

Fixes #17164. Simply checking whether one type is gradually equivalent to another is too simplistic here: `Any` is gradually equivalent to `Todo`, but we should permit users to cast from `Todo` or `Unknown` to `Any` without complaining about it. This changes our logic so that we only complain about redundant casts if:
- the two types are exactly equal (when normalized) OR they are equivalent (we'll still complain about `Any -> Any` casts, and about `Any | str | int` -> `str | int | Any` casts, since their normalized forms are exactly equal, even though the type is not fully static -- and therefore does not participate in equivalence relations)
- AND the casted type does not contain `Todo`

---

_Label `red-knot` added by @AlexWaygood on 2025-04-03 11:34_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-03 11:34_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-03 11:34_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-03 11:34_

---

_Comment by @github-actions[bot] on 2025-04-03 11:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scrapy (https://github.com/scrapy/scrapy)
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/scrapy/scrapy/settings/__init__.py:511:22: Value is already of type `@Todo(generics)`
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/scrapy/scrapy/core/scheduler.py:369:20: Value is already of type `@Todo(generics)`
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/scrapy/scrapy/middleware.py:120:19: Value is already of type `@Todo(generics)`
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/scrapy/scrapy/middleware.py:126:19: Value is already of type `@Todo(generics)`
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/scrapy/scrapy/extensions/httpcache.py:310:16: Value is already of type `@Todo(generics)`
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/scrapy/scrapy/extensions/httpcache.py:392:20: Value is already of type `@Todo(generics)`
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/scrapy/scrapy/extensions/feedexport.py:665:13: Value is already of type `@Todo(generics)`
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/scrapy/scrapy/pipelines/media.py:171:15: Value is already of type `@Todo(generics)`
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/scrapy/scrapy/pipelines/files.py:202:16: Value is already of type `@Todo(generics)`
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/scrapy/scrapy/pipelines/files.py:319:16: Value is already of type `@Todo(generics)`
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/scrapy/scrapy/pipelines/files.py:411:16: Value is already of type `@Todo(generics)`
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/scrapy/scrapy/core/spidermw.py:166:28: Value is already of type `@Todo(generics)`
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/scrapy/scrapy/core/spidermw.py:288:13: Value is already of type `@Todo(generics)`
- Found 1501 diagnostics
+ Found 1488 diagnostics

```
</details>


---

_@sharkdp approved on 2025-04-03 13:48_

Thank you!

---

_Merged by @AlexWaygood on 2025-04-03 14:00_

---

_Closed by @AlexWaygood on 2025-04-03 14:00_

---

_Branch deleted on 2025-04-03 14:00_

---
