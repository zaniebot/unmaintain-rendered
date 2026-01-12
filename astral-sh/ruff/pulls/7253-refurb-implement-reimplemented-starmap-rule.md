```yaml
number: 7253
title: "[refurb] Implement `reimplemented-starmap` rule (`FURB140`)"
type: pull_request
state: merged
author: SavchenkoValeriy
labels:
  - rule
assignees: []
merged: true
base: main
head: refurb/furb140
created_at: 2023-09-09T14:45:27Z
updated_at: 2023-09-19T02:26:53Z
url: https://github.com/astral-sh/ruff/pull/7253
synced_at: 2026-01-12T15:55:23Z
```

# [refurb] Implement `reimplemented-starmap` rule (`FURB140`)

---

_@SavchenkoValeriy_

## Summary

This PR is part of a bigger effort of re-implementing `refurb` rules #1348. It adds support for [FURB140](https://github.com/dosisod/refurb/blob/master/refurb/checks/itertools/use_starmap.py)

## Test Plan

I included a new test + checked that all other tests pass.


---

_Comment by @github-actions[bot] on 2023-09-09 15:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @MichaReiser on `crates/ruff/src/rules/refurb/rules/reimplemented_starmap.rs`:77 on 2023-09-11 07:06_

The get prefix isn't common in Rust. Instead, it is best practice to omit it
```suggestion
    fn element(&self) -> &Expr;
    /// Get generator comprehensions.
    fn generators(&self) -> &[ast::Comprehension];
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/refurb/rules/reimplemented_starmap.rs`:85 on 2023-09-11 07:08_

The use of a generic here means that Rust generates a dedicated implementation for each `T` instantiation. 

I think it may be better to define a `AnyComprehension` enum (or similar) with a variant for each node that you want to support instead of using a trait.  See 

https://github.com/astral-sh/ruff/blob/c05e4628b1d385d106e7e3d355f5d1d76fbfe401/crates/ruff_python_formatter/src/expression/expr_yield.rs#L10-L35

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/refurb/rules/reimplemented_starmap.rs`:130 on 2023-09-11 07:09_

Nit
```suggestion
    let mut diagnostic = Diagnostic::new(ReimplementedStarmap, generator.range());
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/refurb/rules/reimplemented_starmap.rs`:131 on 2023-09-11 07:11_

Nit: Could `check_fix_possibility` and `make_suggestion` be combined so that the enum has a method `try_fix` which returns an `Option` instead? 

---

_@MichaReiser reviewed on 2023-09-11 07:12_

---

_Comment by @MichaReiser on 2023-09-11 07:12_

I have a few coding nits. Leaving it to @charliermarsh to review the rule logic

---

_@SavchenkoValeriy reviewed on 2023-09-11 22:00_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/reimplemented_starmap.rs`:130 on 2023-09-11 22:00_

Gotcha!

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/reimplemented_starmap.rs`:77 on 2023-09-11 22:00_

Will do

---

_@SavchenkoValeriy reviewed on 2023-09-11 22:00_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/reimplemented_starmap.rs`:131 on 2023-09-11 22:01_

Yeah, I see what you mean. Will do!

---

_@SavchenkoValeriy reviewed on 2023-09-11 22:01_

---

_@SavchenkoValeriy reviewed on 2023-09-11 22:10_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/reimplemented_starmap.rs`:85 on 2023-09-11 22:10_

I'm not a huge fan of such a solution. With the trait, we don't need to perform any runtime checks for the given type. With the enum, we do them 4 times: `element`, `generators`, `range`, and `try_fix`. These seem not critical in any way, but one can argue that bloating from instantiating the same (rather small) generic 3 times is kind of similar.

I guess I can see how an enum solution is better for decoupling, if I remove `try_fix` from that interface altogether. Then it can be used in other places that share comprehensions. On the other hand, this interface can't include `DictComprehension` without making `element` optional and adding more runtime checks.

Sorry, for the rant. If you insist, I can change it, but I hope you see my point here 😄 

---

_Label `rule` added by @charliermarsh on 2023-09-11 22:16_

---

_Comment by @codspeed-hq[bot] on 2023-09-16 18:27_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/SavchenkoValeriy:refurb/furb140)

### Merging #7253 will **degrade performances by 2.38%**

<sub>Comparing <code>SavchenkoValeriy:refurb/furb140</code> (ea69b1c) with <code>main</code> (c6ba7df)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/SavchenkoValeriy:refurb/furb140)._

### Benchmarks breakdown

|     | Benchmark | `main` | `SavchenkoValeriy:refurb/furb140` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 34.2 ms | 35.1 ms | -2.38% |


---

_Merged by @charliermarsh on 2023-09-19 02:18_

---

_Closed by @charliermarsh on 2023-09-19 02:18_

---
