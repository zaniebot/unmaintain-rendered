```yaml
number: 19140
title: "[ty] `dataclasses.field` support"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/dataclass-fields
created_at: 2025-07-04T08:19:59Z
updated_at: 2025-07-09T07:23:26Z
url: https://github.com/astral-sh/ruff/pull/19140
synced_at: 2026-01-12T15:56:33Z
```

# [ty] `dataclasses.field` support

---

_@sharkdp_

## Summary

Add an initial set of tests for `dataclasses.field`.

---

_Label `ty` added by @sharkdp on 2025-07-04 08:20_

---

_Comment by @github-actions[bot] on 2025-07-04 08:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo fields = ~156MB
+     memo fields = ~142MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~156MB
+     memo fields = ~171MB

mkdocs (https://github.com/mkdocs/mkdocs)
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~117MB

isort (https://github.com/pycqa/isort)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~80MB

```
</details>


---

_@abhijeetbodas2001 reviewed on 2025-07-08 05:03_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/resources/mdtest/dataclasses/fields.md`:35 on 2025-07-08 05:03_

I thought this was because of a different issue in the overload evaluation code itself? Ref https://github.com/astral-sh/ty/issues/267#issuecomment-3006905373

---

_@sharkdp reviewed on 2025-07-09 06:36_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses/fields.md`:35 on 2025-07-09 06:36_

Thanks, forgot about that. Not sure if it's really the same though? The overloads for `dataclasses.field` look different from the ones for `attrs.field`. Here's a simplified version of the `dataclasses.field` overloads:

https://play.ty.dev/c5424d2e-ac73-4759-a31e-e3fb4eed1420

If I change that to use a simple class as a sentinel value, instead of an enum (for which we infer a dynamic `@Todo` type), it works fine:

https://play.ty.dev/3d16e359-69db-4265-b253-5c1d6d0e3b3b

---

_Label `internal` added by @sharkdp on 2025-07-09 07:16_

---

_Label `testing` added by @sharkdp on 2025-07-09 07:16_

---

_Marked ready for review by @sharkdp on 2025-07-09 07:16_

---

_Review requested from @carljm by @sharkdp on 2025-07-09 07:16_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-09 07:16_

---

_Review requested from @dcreager by @sharkdp on 2025-07-09 07:16_

---

_Comment by @sharkdp on 2025-07-09 07:17_

I guess it doesn't hurt to have these tests in place, so we'll notice when something changes in type inference for the return type of `field`. Also, @abhijeetbodas2001 plans to work on dataclass fields, so this seems like a good place to start.

---

_Merged by @sharkdp on 2025-07-09 07:18_

---

_Closed by @sharkdp on 2025-07-09 07:18_

---

_Branch deleted on 2025-07-09 07:18_

---

_@abhijeetbodas2001 reviewed on 2025-07-09 07:23_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/resources/mdtest/dataclasses/fields.md`:35 on 2025-07-09 07:23_

Oh yes, these indeed look to be different issues. It seems I was conflating `attrs.field` and `dataclasses.field`. Thank you for the examples!

The issue I linked above will still come to bite us later on though, since `ty` should ideally also understand `attrs.field`.

---
