```yaml
number: 3396
title: "Use shared `leading_quote` implementation in ruff_python_formatter"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/string-helper
created_at: 2023-03-08T00:44:05Z
updated_at: 2023-03-08T18:22:01Z
url: https://github.com/astral-sh/ruff/pull/3396
synced_at: 2026-01-12T15:55:12Z
```

# Use shared `leading_quote` implementation in ruff_python_formatter

---

_@charliermarsh_

_No description provided._

---

_Review comment by @charliermarsh on `crates/ruff_python_stdlib/src/str.rs`:15 on 2023-03-08 00:44_

(Drive-by fix -- this is no longer used anywhere, but unrelated to the change at hand.)

---

_@charliermarsh reviewed on 2023-03-08 00:44_

---

_@MichaReiser reviewed on 2023-03-08 08:33_

---

_Review comment by @MichaReiser on `crates/ruff_python_stdlib/src/str.rs`:15 on 2023-03-08 08:33_

I noticed that we use `pub` in many places where it isn't necessary. It would be good in my view, to be as restrictive as possible when it comes to defining visibility. Not using `pub` has the advantage that Rust flags unused symbols.

---

_@MichaReiser approved on 2023-03-08 08:34_

---

_@MichaReiser reviewed on 2023-03-08 08:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/strings.rs`:3 on 2023-03-08 08:37_

What's the reason that `raw_contents` is defined in the AST crate vs in the `stdlib` crate? Defining all symbols in the same crates would have the advantage that it may not be necessary for `SINGLE_QUOTE_PREFIXES` to be public. 

Maybe the more fundamental question is what's the difference between the `stdlib` and `ast` crates and is it worth having both or should we merge them into a single `python_syntax` crate? If not, could we add a README to each create (or documentation to `lib.rs`) explaining the intended purpose of each crate. 

---

_@charliermarsh reviewed on 2023-03-08 17:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/strings.rs`:3 on 2023-03-08 17:55_

I think these should actually be moved into the `ast` crate.

The `stdlib` crate was intended to serve as roughly 1:1 reimplementations of functionality that you typically get from the Python standard library (e.g., lists of keywords, checks for whether something is a valid identifier, lists of standard library modules, and so on).

---

_Merged by @charliermarsh on 2023-03-08 18:22_

---

_Closed by @charliermarsh on 2023-03-08 18:22_

---

_Branch deleted on 2023-03-08 18:22_

---
