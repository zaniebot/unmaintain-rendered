```yaml
number: 2071
title: "fix: issue D401 only for non-test/property functions and methods"
type: pull_request
state: merged
author: scop
labels: []
assignees: []
merged: true
base: main
head: feat/d401-improvements
created_at: 2023-01-21T21:24:26Z
updated_at: 2023-01-23T08:11:25Z
url: https://github.com/astral-sh/ruff/pull/2071
synced_at: 2026-01-12T04:52:00Z
```

# fix: issue D401 only for non-test/property functions and methods

---

_Pull request opened by @scop on 2023-01-21 21:24_

Extend test fixture to verify the targeting.

Includes two "attribute docstrings" which per PEP 257 are not recognized by the Python bytecode compiler or available as runtime object attributes. They are not available for us either at time of writing, but include them for completeness anyway in case they one day are.

Rust newbie here, review with a fine comb greatly appreciated :)

---

_Review comment by @charliermarsh on `src/rules/pydocstyle/rules/non_imperative_mood.rs`:36 on 2023-01-21 22:58_

We have a little helper for this --- `cast::name` from `src/ast/cast.rs`. We know these are functions, even though it's not encoded in the type system. (Not _great_, but we should definitely error anyway if we get a non-`FunctionDef` here.)

---

_Review comment by @charliermarsh on `src/rules/pydocstyle/rules/non_imperative_mood.rs`:42 on 2023-01-21 23:01_

It looks like Pydocstyle defaults to accepting `property,cached_property,functools.cached_property`.

Maybe just for consistency with this "kind" of check, let's add a method to `src/visibilty.rs` like:

```rs
pub fn is_property(checker: &Checker, decorator_list: &[Expr]) -> bool {
    decorator_list.iter().any(|expr| {
        checker.resolve_call_path(expr).map_or(false, |call_path| {
            call_path.as_slice() == ["", "property"] || call_path.as_slice() == ["functools", "cached_property"]
        })
    })
}
```

That will check the additional decorator _and_ ensure that it only triggers when `functools` is imported _and_ track aliases _and_ track overrides :)

---

_@charliermarsh requested changes on 2023-01-21 23:02_

Great! Let me know if any of the below is unclear.

---

_@scop reviewed on 2023-01-21 23:56_

---

_Review comment by @scop on `src/rules/pydocstyle/rules/non_imperative_mood.rs`:42 on 2023-01-21 23:56_

I actually implemented `is_property` in `visibility.rs` in my first cut of this patch, but then decided to move it back, "inlined" at the call site. I felt `@property` isn't actually related to visibility in the broad sense the other similar helpers there are, and thus it seemed misplaced.

But no strong opinions really, could you confirm if you'd still rather see it there and I'll refactor.

In any case, will revise this tomorrow. Thanks!

---

_@charliermarsh reviewed on 2023-01-21 23:59_

---

_Review comment by @charliermarsh on `src/rules/pydocstyle/rules/non_imperative_mood.rs`:42 on 2023-01-21 23:59_

Yeah I think that's a totally reasonable instinct, but I'd still rather see it in there. There are some other helpers in that file that aren't _really_ related to visibility (like `is_override`), and I'd like to have them logically grouped for now.

---

_@scop reviewed on 2023-01-22 19:22_

---

_Review comment by @scop on `src/rules/pydocstyle/rules/non_imperative_mood.rs`:36 on 2023-01-22 19:22_

Made use of that and `cast::decorator_list` in 05ec78c0812d8657442a6cd4900dfd235da17b50.

---

_@scop reviewed on 2023-01-22 19:22_

---

_Review comment by @scop on `src/rules/pydocstyle/rules/non_imperative_mood.rs`:42 on 2023-01-22 19:22_

Sure, done in f38b51a29dc243060f77c6d90e6e453242f1ef7e.

---

_Comment by @charliermarsh on 2023-01-22 19:22_

Looks great! Will merge when CI is green.

---

_@charliermarsh approved on 2023-01-22 19:24_

---

_Merged by @charliermarsh on 2023-01-22 19:25_

---

_Closed by @charliermarsh on 2023-01-22 19:25_

---

_Comment by @charliermarsh on 2023-01-22 19:25_

Thanks tons.

---

_Branch deleted on 2023-01-22 20:40_

---

_Comment by @akx on 2023-01-23 08:11_

@scop Thanks for refining this! ❤️ 

---
