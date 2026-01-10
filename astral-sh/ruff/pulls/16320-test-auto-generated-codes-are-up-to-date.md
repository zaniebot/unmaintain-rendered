```yaml
number: 16320
title: Test auto generated codes are up to date
type: pull_request
state: closed
author: Glyphack
labels:
  - ci
  - testing
assignees: []
base: main
head: move-generated-test-check-to-code
created_at: 2025-02-22T20:44:45Z
updated_at: 2025-02-25T12:37:15Z
url: https://github.com/astral-sh/ruff/pull/16320
synced_at: 2026-01-10T19:49:01Z
```

# Test auto generated codes are up to date

---

_Pull request opened by @Glyphack on 2025-02-22 20:44_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

This is just an idea feel free to close the PR if you don't see the benefit. I thought it would be quicker to discuss in the PR rather than issue.

## Summary

This PR replaces the check in the CI to ensure auto generated codes are up to date with a rust test. The benefit is that this will run along the tests and fails if the auto generated code is not up to date.

I was checking rust analyzer ast generator setup, and noticed [this test](https://github.com/rust-lang/rust-analyzer/blob/862693ff954660d169774c8ada4672fb96dabf36/crates/syntax/src/tests/sourcegen_ast.rs#L21) that checks for generate scripts to be up to date.
I think it's a nice improvement since the check we have now only runs in the CI, when I was working on the ast auto generation I forgot to update the generated file and found out in the CI.

Changes to make this work:

- Installing `uv` before running test in all steps. This is because we need to ensure we're running 3.11.
- line endings have different representation in different platforms on windows it's `\r\n` so we need to replace that with `\n` before checking. I have to admit this one was tricky for me to figure out why is it broken. Maybe it worth it to print a minimal diff in the CI?

The CI time does not seem to increase with this change.
[this branch](https://github.com/astral-sh/ruff/actions/runs/13475837412/job/37654902156) vs [main](https://github.com/astral-sh/ruff/actions/runs/13475837412/job/37654902156)

## Test Plan

Moved the CI check to a rust test. The failure looks like this:

```
ruff/crates/ruff_python_ast/src/generated.rs has changed after running ruff/crates/ruff_python_ast/generate.py. Commit the changes.
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

<!-- How was it tested? -->


---

_Converted to draft by @Glyphack on 2025-02-22 20:48_

---

_Comment by @github-actions[bot] on 2025-02-22 21:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @Glyphack on 2025-02-22 22:22_

---

_Label `ci` added by @AlexWaygood on 2025-02-22 22:43_

---

_Label `testing` added by @AlexWaygood on 2025-02-22 22:43_

---

_Comment by @MichaReiser on 2025-02-23 09:40_

I like the idea of running those checks as part of tests. This is actually similar to how we already assert that the JSON schemas are up to date and it has the benefit that we get signal earlier.

However, its dependency on uv does unnecessarily complicate running tests locally (I now have to ensure that my uv environment is up to date). 

That's why I think a prerequisite for this is that we rewrite all Python scripts into Rust so that they are dependency-free.

---

_Comment by @Glyphack on 2025-02-23 13:51_

I close it to revisit afterwards when we update the code.

---

_Closed by @Glyphack on 2025-02-23 13:51_

---

_Branch deleted on 2025-02-25 12:37_

---
