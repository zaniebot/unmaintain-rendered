```yaml
number: 11644
title: "[`flake8-bugbear`] Implement `return-in-generator` (`B901`)"
type: pull_request
state: merged
author: tobb10001
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: b901-return-x-generator-function
created_at: 2024-05-31T17:58:59Z
updated_at: 2024-06-01T11:38:40Z
url: https://github.com/astral-sh/ruff/pull/11644
synced_at: 2026-01-10T21:56:00Z
```

# [`flake8-bugbear`] Implement `return-in-generator` (`B901`)

---

_Pull request opened by @tobb10001 on 2024-05-31 17:58_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR implements the rule B901, which is part of the opinionated rules of `flake8-bugbear`.

This rule seems to be desired in `ruff` as per https://github.com/astral-sh/ruff/issues/3758 and https://github.com/astral-sh/ruff/issues/2954#issuecomment-1441162976.

## Test Plan

As this PR was made closely following the [CONTRIBUTING.md](https://github.com/astral-sh/ruff/blob/8a25531a7144fd4a6b62c54efde1ef28e2dc18c4/CONTRIBUTING.md), it tests using the snapshot approach, that is described there.

## Sources

The implementation is inspired by [the original implementation in the `flake8-bugbear` repository](https://github.com/PyCQA/flake8-bugbear/blob/d1aec4cbef7c4a49147c428b7e4a97e497b5d163/bugbear.py#L1092). The error message and [test file](https://github.com/PyCQA/flake8-bugbear/blob/d1aec4cbef7c4a49147c428b7e4a97e497b5d163/tests/b901.py) where also copied from there.

The documentation I came up with on my own and needs improvement. Maybe the example given in https://github.com/astral-sh/ruff/issues/2954#issuecomment-1441162976 could be used, but maybe they are too complex, I'm not sure.

## Open Questions

- [x] Documentation. (See above.)

- [x] Can I access the parent in a visitor?

    The [original implementation](https://github.com/PyCQA/flake8-bugbear/blob/d1aec4cbef7c4a49147c428b7e4a97e497b5d163/bugbear.py#L1100) references the `yield` statement's parent to check if it is an expression statement. I didn't find a way to do this in `ruff` and used the `is_expresssion_statement` field on the visitor instead. What are your thoughts on this? Is it possible and / or desired to access the parent node here?

- [x] Is `Option::is_some(...)` -> `...unwrap()` the right thing to do?

    Referring to [this piece of code](https://github.com/tobb10001/ruff/blob/9d5a280f71103ef33df5676d00a6c68c601261ac/crates/ruff_linter/src/rules/flake8_bugbear/rules/return_x_in_generator.rs?plain=1#L91-L96). From my understanding, the `.unwrap()` is safe, because it is checked that `return_` is not `None`. However, I feel like I missed a more elegant solution that does both in one.

## Other

I don't know a lot about this rule, I just implemented it because I found it in a https://github.com/astral-sh/ruff/labels/good%20first%20issue.

I'm new to Rust, so any constructive critisism is appreciated.

---

_Comment by @github-actions[bot] on 2024-05-31 18:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 2 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/93e6f0070aa4d295f348912c10037be63c419e0f/tests/models/test_taskinstance.py#L2421'>tests/models/test_taskinstance.py:2421:17:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/16fc3936a4005513c6afebfbbf7919445d7032ac/integration/test_server_side_event.py#L24'>integration/test_server_side_event.py:24:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B901 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_@charliermarsh reviewed on 2024-05-31 19:08_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_x_in_generator.rs`:91 on 2024-05-31 19:08_

Typically you'd write this like:

```rust
if visitor.has_yield {
  if let Some(return_) = visitor.return_.as_ref() {
    checker.diagnositcs.push(Diagnostic::new(ReturnXInGenerator, return_));
  }
}
```

That way, you can avoid the unwrap by making it _guaranteed_ that you have a `Some` if the condition passes.

---

_@charliermarsh reviewed on 2024-05-31 19:08_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_x_in_generator.rs`:63 on 2024-05-31 19:08_

I think I would do this by checking `Stmt::Expr` here, and then just introspecting the value directly to see if the value is `Expr::Yield`.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-31 19:08_

---

_Label `rule` added by @charliermarsh on 2024-05-31 19:09_

---

_Label `preview` added by @charliermarsh on 2024-05-31 19:09_

---

_@charliermarsh reviewed on 2024-05-31 19:09_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_x_in_generator.rs`:91 on 2024-05-31 19:09_

(Separately, `visitor.return_.is_some()` would be more natural than `Option::is_some` -- we can call it as a method.)

---

_Comment by @charliermarsh on 2024-05-31 19:09_

Thanks! Two small comments based on your questions.

---

_Review comment by @tobb10001 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_x_in_generator.rs`:91 on 2024-05-31 19:38_

Thanks for the hint.

The compiler didn't like `.as_ref()`, so I just left it out.

---

_@tobb10001 reviewed on 2024-05-31 19:38_

---

_@tobb10001 reviewed on 2024-05-31 19:39_

---

_Review comment by @tobb10001 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_x_in_generator.rs`:63 on 2024-05-31 19:39_

Good idea, I updated the implementation to this.

---

_@charliermarsh reviewed on 2024-05-31 20:23_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_x_in_generator.rs`:91 on 2024-05-31 20:23_

Oh yeah, if you don't need it def omit -- sometimes you need it, was hard to tell when writing on GitHub :)

---

_@charliermarsh approved on 2024-05-31 21:39_

Thanks!

---

_Renamed from "[`flake8-bugbear`] Implement `return-x-in-generator` (`B901`)" to "[`flake8-bugbear`] Implement `return-in-generator` (`B901`)" by @charliermarsh on 2024-05-31 21:39_

---

_Merged by @charliermarsh on 2024-05-31 21:48_

---

_Closed by @charliermarsh on 2024-05-31 21:48_

---
