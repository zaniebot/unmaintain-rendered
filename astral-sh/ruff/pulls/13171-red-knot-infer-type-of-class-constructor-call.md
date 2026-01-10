```yaml
number: 13171
title: "[red-knot] Infer type of class constructor call expression"
type: pull_request
state: merged
author: dylwil3
labels:
  - ty
assignees: []
merged: true
base: main
head: class-constructor
created_at: 2024-08-30T22:54:51Z
updated_at: 2024-09-05T15:27:15Z
url: https://github.com/astral-sh/ruff/pull/13171
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Infer type of class constructor call expression

---

_Pull request opened by @dylwil3 on 2024-08-30 22:54_

This tiny PR implements the following type inference: the type of `Foo(...)` will be `Foo`.


---

_Review requested from @carljm by @dylwil3 on 2024-08-30 22:54_

---

_Review requested from @MichaReiser by @dylwil3 on 2024-08-30 22:54_

---

_Review requested from @AlexWaygood by @dylwil3 on 2024-08-30 22:54_

---

_Comment by @github-actions[bot] on 2024-08-30 23:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:331 on 2024-08-30 23:21_

Sooo... I think we should keep _some_ TODO comment here, because there are unfortunately some edge cases we'll need to handle in due course:
- Classes with `__new__` methods that are annotated as not returning an instance of the class
- Classes where the metaclass has a __call__ method with an unusual return annotation

These are edge cases that we _definitely_ don't need to worry about now, but a TODO comment would be great IMO

---

_@AlexWaygood approved on 2024-08-30 23:21_

Looks great!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:331 on 2024-08-30 23:25_

```suggestion
            // TODO annotated return type on `__new__` or metaclass `__call__`
            Type::Class(class) => Some(Type::Instance(*class)),
```

---

_@carljm approved on 2024-08-30 23:26_

Awesome, thank you!! This looks perfect other than the one TODO comment, so I'll just apply that change myself, let the tests run, and then merge.

---

_@carljm reviewed on 2024-08-30 23:29_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:331 on 2024-08-30 23:29_

On second look I made one other small change here. We don't need to match again in `self.instance` when we've already matched once and know we have a `Type::Class` -- the definition of `instance()` for `Type::Class` is quite obvious and short, it's fine to repeat it here. The main purpose of `instance()` is to abstract across `Type` variants, which we don't need here.

---

_Comment by @codspeed-hq[bot] on 2024-08-30 23:35_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dylwil3:class-constructor)

### Merging #13171 will **degrade performances by 8.72%**

<sub>Comparing <code>dylwil3:class-constructor</code> (b7e47a9) with <code>main</code> (28ab5f4)</sub>



### Summary

`❌ 1 (👁 1)` regressions
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dylwil3:class-constructor` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `linter/all-with-preview-rules[numpy/globals.py]` | 819.6 µs | 897.9 µs | -8.72% |


---

_Comment by @carljm on 2024-08-30 23:36_

CodSpeed regression is false alarm :/ acknowledging on CodSpeed.

---

_Merged by @carljm on 2024-08-30 23:48_

---

_Closed by @carljm on 2024-08-30 23:48_

---

_Label `red-knot` added by @dhruvmanila on 2024-09-05 15:27_

---
