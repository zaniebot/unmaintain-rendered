```yaml
number: 5192
title: Upgrade RustPython
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/upgrade
created_at: 2023-06-19T19:16:18Z
updated_at: 2023-06-19T21:13:56Z
url: https://github.com/astral-sh/ruff/pull/5192
synced_at: 2026-01-12T03:43:30Z
```

# Upgrade RustPython

---

_Pull request opened by @charliermarsh on 2023-06-19 19:16_

## Summary

This PR upgrade RustPython to pull in the changes to `Arguments` (zip defaults with their identifiers) and all the renames to `CmpOp` and friends.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-19 19:25_

---

_Review requested from @konstin by @charliermarsh on 2023-06-19 19:25_

---

_Review comment by @MichaReiser on `Cargo.lock`:202 on 2023-06-19 19:46_

So many new dependencies. Are they all introduced because of the bigint library migration? Would it make sense for us to stay on numbigint for now?

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:1768 on 2023-06-19 19:47_

Hmm another acronym with `def`. I should have catched that during the review. I guess that's fine for now.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bandit/rules/hardcoded_password_default.rs`:50 on 2023-06-19 19:49_

What's the motivation for using `chain!` over `iter.chain`? I know we have a few use cases for `itertools` but I try to keep them to a minimum, in the hope, that we could someday remove the dependency.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_boolean_trap/rules/check_positional_boolean_in_def.rs`:101 on 2023-06-19 19:51_

Should we add some custom methods to `arguments` that allow you iterating over all arguments, over all defaults, etc? It feels very repetitive to have these `chain` calls sprinkled across the code base.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/unexpected_special_method_signature.rs`:164 on 2023-06-19 19:54_

Can we change this to count the `mandatory` params directly?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor.rs`:37 on 2023-06-19 19:55_

```suggestion
    fn visit_bool_op(&mut self, boolop: &'a BoolOp) {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor.rs`:43 on 2023-06-19 19:56_

```suggestion
    fn visit_unary_op(&mut self, unaryop: &'a UnaryOp) {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor.rs`:46 on 2023-06-19 19:56_

```suggestion
    fn visit_cmp_op(&mut self, cmpop: &'a CmpOp) {
```

Same for `walk_`

... and other visitor functions. Also applies to the preorder visitor.

---

_@MichaReiser approved on 2023-06-19 19:59_

Thank you! 

This looks good to me. I am a bit concerned about all the new dependencies that the upgrade introduces and wonder if it could make sense to add helpers to `Arguments` to avoid the many `chain` in the user code (get all defaults, get all named arguments, get all positional arguments, get all arguments) etc.

---

_@charliermarsh reviewed on 2023-06-19 20:05_

---

_Review comment by @charliermarsh on `Cargo.lock`:202 on 2023-06-19 20:05_

I'm not sure, but we _are_ staying on num-bigint, aren't we? I'm not using the malachite feature.

---

_@charliermarsh reviewed on 2023-06-19 20:05_

---

_Review comment by @charliermarsh on `Cargo.lock`:202 on 2023-06-19 20:05_

It does look like `block-buffer` comes from `malachite`. But does this actually affect the binary? I can also just remove the `malachite` stuff in a separate PR.

---

_@charliermarsh reviewed on 2023-06-19 20:07_

---

_Review comment by @charliermarsh on `Cargo.lock`:202 on 2023-06-19 20:07_

Oh, I think `rustpython_format` doesn't have this properly gated as a feature. I'll fix this separately.

---

_@charliermarsh reviewed on 2023-06-19 20:07_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/rules/hardcoded_password_default.rs`:50 on 2023-06-19 20:07_

I can change it to `iter.chain`. I just wanted to be consistent everywhere and this was more concise.

---

_Comment by @github-actions[bot] on 2023-06-19 20:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `Cargo.lock`:202 on 2023-06-19 20:27_

Ok, fixed this. No new deps.

---

_@charliermarsh reviewed on 2023-06-19 20:27_

---

_@charliermarsh reviewed on 2023-06-19 20:28_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1768 on 2023-06-19 20:28_

Yeah don't love it.

---

_@MichaReiser reviewed on 2023-06-19 20:35_

---

_Review comment by @MichaReiser on `Cargo.lock`:202 on 2023-06-19 20:35_

Thank you!

---

_@charliermarsh reviewed on 2023-06-19 20:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_boolean_trap/rules/check_positional_boolean_in_def.rs`:101 on 2023-06-19 20:58_

Yeah, maybe, I'm gonna punt it on now under "leave it better" (it's no worse than before).

---

_Merged by @charliermarsh on 2023-06-19 21:09_

---

_Closed by @charliermarsh on 2023-06-19 21:09_

---

_Branch deleted on 2023-06-19 21:09_

---
