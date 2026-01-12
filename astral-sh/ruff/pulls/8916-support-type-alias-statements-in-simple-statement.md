```yaml
number: 8916
title: Support type alias statements in simple statement positions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: charlie/soft-keyword
created_at: 2023-11-30T00:55:43Z
updated_at: 2023-11-30T19:25:40Z
url: https://github.com/astral-sh/ruff/pull/8916
synced_at: 2026-01-10T23:40:55Z
```

# Support type alias statements in simple statement positions

---

_Pull request opened by @charliermarsh on 2023-11-30 00:55_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Our `SoftKeywordTokenizer` only respected soft keywords in compound statement positions -- for example, at the start of a logical line:

```python
type X = int
```

However, type aliases can also appear in simple statement positions, like:

```python
class Class: type X = int
```

(Note that `match` and `case` are _not_ valid keywords in such positions.)

This PR upgrades the tokenizer to track both kinds of valid positions.

Closes https://github.com/astral-sh/ruff/issues/8900.
Closes https://github.com/astral-sh/ruff/issues/8899.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-11-30 00:55_

---

_Review requested from @zanieb by @charliermarsh on 2023-11-30 00:55_

---

_Label `bug` added by @charliermarsh on 2023-11-30 00:55_

---

_Label `parser` added by @charliermarsh on 2023-11-30 00:55_

---

_@charliermarsh reviewed on 2023-11-30 00:56_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/soft_keywords.rs`:223 on 2023-11-30 00:56_

This and `Nested` could be combined, since this is just `Nested` with a depth of `0`. Alternatively, `Nested` could be `NonZeroU32`. But both felt clunkier in practice. Open to changing, though.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/soft_keywords.rs`:167 on 2023-11-30 00:58_

Nit: You could implement `Eq` on `Position` so that you can use `==` instead of `matches`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/soft_keywords.rs`:165 on 2023-11-30 00:58_

Just looking at the colon itself might not be sufficient because colons are also used to separate type annotations, slice indices, etc). I'm not sure if this is an issue here, but let's add a few more tests, e.g. `a: type X = int` (which should not be valid)

---

_Comment by @github-actions[bot] on 2023-11-30 01:11_

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

_Review comment by @zanieb on `crates/ruff_python_parser/src/parser.rs`:828 on 2023-11-30 05:18_

I'm confused that you call these compound statements here but simple statements everywhere else.

---

_@zanieb reviewed on 2023-11-30 05:18_

---

_Review requested from @zanieb by @charliermarsh on 2023-11-30 05:35_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/soft_keywords.rs`:100 on 2023-11-30 07:09_

Unrelated to your change: It's somewhat unfortunate that it's necessary to track whether we're at the start of the line, because we already know this inside of the `Lexer`. 

---

_@MichaReiser approved on 2023-11-30 07:13_

I overall like the solution. I'm a bit worried that the `colon` detection is a bit too permissive.

---

_@MichaReiser reviewed on 2023-11-30 07:14_

I overall like the solution. I'm a bit worried that the `colon` detection is a bit too permissive.

---

_@charliermarsh reviewed on 2023-11-30 15:57_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/soft_keywords.rs`:165 on 2023-11-30 15:57_

In the logical line rules, we have some more extensive logic to ensure that the colon follows a statement keyword (like `class`). I could add that here...

---

_@charliermarsh reviewed on 2023-11-30 15:59_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/soft_keywords.rs`:100 on 2023-11-30 15:59_

Definitely. Perhaps we could move this into the lexer...

---

_Merged by @charliermarsh on 2023-11-30 19:15_

---

_Closed by @charliermarsh on 2023-11-30 19:15_

---

_Branch deleted on 2023-11-30 19:15_

---
