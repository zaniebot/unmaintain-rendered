```yaml
number: 3787
title: Add import insertion support to autofix capabilities
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/require-imports
created_at: 2023-03-28T22:17:01Z
updated_at: 2023-03-30T15:53:41Z
url: https://github.com/astral-sh/ruff/pull/3787
synced_at: 2026-01-12T04:39:45Z
```

# Add import insertion support to autofix capabilities

---

_Pull request opened by @charliermarsh on 2023-03-28 22:17_

## Summary

We often have rules (and violations) that could in theory be autofixed, but that rely on the presence of an imported symbol that may or may not be available in scope.

The `sys-exit-alias` rule is a good example. It recommends replacing calls to the builtin `exit` and `quit` methods with calls to `sys.exit`. That's "easy" to autofix... as long as `sys.exit` is in-scope.

This PR enables rules to import symbols into the current scope in a fairly general way, enabling autofix support for rules like `sys-exit-alias`. Below, I've also extended autofix support for `lru-cache-with-maxsize-none`, which relies on making `functools.cache` available.

### How it works

From a rule's perspective, the end-user API leans on a single `get_or_import_symbol` method, which returns (1) an `Edit` (to "insert" the symbol into the scope) and (2) the bound name of the resolved symbol (e.g., `exit`, if we added `from sys import exit`, or `sys.exit`, if we added `import sys`).

Under-the-hood, that method performs a series of lookups:

1. First, it checks to see if `sys.exit` is _already_ available in any form (e.g., `from sys import exit as foo`, `import sys as foo`); if so, it returns a reference to it.
2. Otherwise, it checks to see if there's an `import ... from` for the relevant module (e.g., `from sys import path`). If so, it modifies that import to include the necessary symbol (`from sys import path, exit`). If `exit` is already bound to something else, it aborts and logs an error.
3. Otherwise, it generates an edit to insert an `import sys`, and returns `sys.exit` as the bound reference.

The logic for inserting imports is crude and follows LibCST's own logic: if there are no imports, we insert at top-of-file; otherwise, we insert after the last-seen top-level import statement.

### Limitations

There are a few limitations to the current approach:

1. We don't attempt to retain import order at all. We leave that up to the `isort` rules (or `isort` itself, if you're using `isort`).

2. We _only_ respect top-level imports, so here, we'd still insert an `import sys`:

```py
def f():
  from sys import path

  quit()
```

3. Every individual fix (that relies on import insertion) has to go through this process. We may want to implement some sort of cache here.

Closes #835.


---

_Review requested from @konstin by @charliermarsh on 2023-03-28 22:52_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-28 22:52_

---

_Marked ready for review by @charliermarsh on 2023-03-28 22:52_

---

_Comment by @github-actions[bot] on 2023-03-28 22:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.34ms     2.4 MB/sec    1.04     17.8±0.36ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.12ms     3.9 MB/sec    1.03      4.5±0.16ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   572.2±15.38µs     5.2 MB/sec    1.00   573.5±14.79µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.25ms     3.5 MB/sec    1.03      7.6±0.16ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.13ms     4.7 MB/sec    1.09      9.4±0.32ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1977.1±55.79µs     8.4 MB/sec    1.03      2.0±0.08ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    235.8±7.58µs    12.5 MB/sec    1.03   242.3±11.21µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.09ms     6.4 MB/sec    1.06      4.3±0.11ms     6.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.7±0.17ms     3.0 MB/sec    1.00     13.7±0.18ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.9 MB/sec    1.00      3.4±0.04ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    358.0±6.67µs     8.2 MB/sec    1.00    357.8±3.90µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.7±0.06ms     4.4 MB/sec    1.00      5.7±0.06ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.07ms     5.6 MB/sec    1.00      7.2±0.07ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1508.0±13.70µs    11.0 MB/sec    1.00  1494.8±18.25µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.1±1.85µs    18.9 MB/sec    1.00    156.3±3.88µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.03ms     7.9 MB/sec    1.00      3.2±0.04ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-03-28 22:54_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer.rs`:145 on 2023-03-28 22:54_

GitHub isn't picking it up, but everything from here down is moved from `rules/isort/rules/add_required_import.rs` (which now uses this `Importer` struct).

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/helpers.rs`:488 on 2023-03-28 22:56_

This piece here is a hack. Consider:

```py
import sys

quit()
```

Assume you _omit_ this hack. If you run with `unused-imports` and `sys-exit-alias` here, then we'll generate two fixes: (1) we'll remove the unused `import sys`; (2) we'll replace `quit()` with `sys.exit()`, under the assumption that `sys` is already imported and available. That'd leave us with an invalid reference.

The workaround is to add a "fake" edit to replace `import sys` with `import sys`. This ensures that we don't try to apply conflicting autofixes to the file.

We may be getting to the point at which we want to avoid trying to apply multiple fixes in the same autofix pass (which would also avoid this issue).


---

_@charliermarsh reviewed on 2023-03-28 22:56_

---

_Comment by @JonathanPlasse on 2023-03-29 05:36_

What happens if `sys`is shadowed?
Do you import `sys` with an alias like `sys_` with as many `_` necessary to avoid the shadowing?

---

_Comment by @charliermarsh on 2023-03-29 13:47_

No, we avoid fixing if the name is already bound to something else. In practice, I think that's the least surprising behavior.

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:205 on 2023-03-30 06:36_

Nit: Surprising that clippy isn't flagging this
```suggestion
            if scope_index.is_global() &&self.ctx.current_stmt_parent().is_none() {
                self.importer.visit_import(stmt);
            }
```

---

_Review comment by @MichaReiser on `crates/ruff/src/cst/matchers.rs`:64 on 2023-03-30 06:37_

What's the reason for using `bail` here instead of returning `Err` and propagating the error (we'll have to replace all `bail`, `warn_once` at some point in the future if we want to build a python API)

---

_Review comment by @MichaReiser on `crates/ruff/src/importer.rs`:54 on 2023-03-30 06:39_

Nit: I would use `panic` here instead of `unreachable` because you use it as an invariant. It isn't that the code above guarantees that this path isn't possible.

---

_@MichaReiser approved on 2023-03-30 06:42_

Nice!

Can we add a test case that demonstrates what happens if we have

```python
if test:
	import path


exit(1)
```

or

```python
import path

if test:
	pass

import numpy

// ...

exit(1)
```

---

_Review comment by @konstin on `crates/ruff/src/autofix/helpers.rs`:488 on 2023-03-30 09:56_

i'd add this to the code comment

---

_Review comment by @konstin on `crates/ruff/src/autofix/helpers.rs`:508 on 2023-03-30 09:59_

why not `from functools import lru_cache`? that's what i'd expect and afaik that's what pycharm generally does

---

_Review comment by @konstin on `crates/ruff/src/cst/matchers.rs`:64 on 2023-03-30 10:03_

(fwiw there's anyhow integration with pyo3 that translates them into `RuntimeError`, but i agree that proper error types are better)

---

_Review comment by @konstin on `crates/ruff/src/importer.rs`:17 on 2023-03-30 10:08_

learning question: why is this type called a suit?

---

_Review comment by @konstin on `crates/ruff/src/importer.rs`:47 on 2023-03-30 10:14_

Nit: i find those easier to read
```suggestion
                if level == Some(0) {
```

---

_Review comment by @konstin on `crates/ruff_python_ast/src/imports.rs`:64 on 2023-03-30 10:31_

i never realized that `from ....bar import foo` valid syntax and now i'm a scared of imports

---

_@konstin approved on 2023-03-30 10:32_

---

_Review comment by @MichaReiser on `crates/ruff/src/cst/matchers.rs`:64 on 2023-03-30 10:33_

Oh, I think I confused what `bail!` does. Does bail return an `Err`? If so, then I think that's fine for now. 

---

_@MichaReiser reviewed on 2023-03-30 10:33_

---

_@konstin reviewed on 2023-03-30 10:47_

---

_Review comment by @konstin on `crates/ruff/src/cst/matchers.rs`:64 on 2023-03-30 10:47_

yes it's just a convenience macro around that: https://docs.rs/anyhow/latest/src/anyhow/macros.rs.html#56-66

---

_@charliermarsh reviewed on 2023-03-30 15:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:205 on 2023-03-30 15:18_

We have that rule disabled, I actually find nested `if` blocks to be much clearer but it's a completely subjective thing 🙈 

---

_@charliermarsh reviewed on 2023-03-30 15:19_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer.rs`:17 on 2023-03-30 15:19_

It comes from RustPython. It's kind of like how a test suite is a collection of tests. `Suite` is just `Vec<Stmt>` IIRC -- so, a collection of statements.

---

_Review comment by @charliermarsh on `crates/ruff/src/importer.rs`:54 on 2023-03-30 15:20_

Let me change this in a separate pass because we do this in a lot of places (but what you're saying makes sense).

---

_@charliermarsh reviewed on 2023-03-30 15:20_

---

_@charliermarsh reviewed on 2023-03-30 15:20_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/imports.rs`:64 on 2023-03-30 15:20_

As you should be!

---

_@charliermarsh reviewed on 2023-03-30 15:23_

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/helpers.rs`:508 on 2023-03-30 15:23_

Yeah I wasn't sure what the default should be here. I was thinking that it should be rule-dependent (e.g., it makes sense to do `from typing import List` rather than `import typing; typing.List` if we need to inject from typing).

Where does PyCharm do this? When you have `@cache` and cache isn't imported? (But in that case, the user has already expressed that they want the member, not the fully-qualified name.)

---

_@charliermarsh reviewed on 2023-03-30 15:24_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer.rs`:47 on 2023-03-30 15:24_

In this case, though, we want to allow `None` or `Some(0)`

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:205 on 2023-03-30 15:26_

It was useful for us because we often had things gated beyond `settings.rules.enabled` checks and it was useful to separate them conceptually, like:

```rust
if settings.rules.enabled(Rule:...) {
  if condition_for_rule {
    do_rule
  }
}
```

---

_@charliermarsh reviewed on 2023-03-30 15:26_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:205 on 2023-03-30 15:27_

Changed here.

---

_@charliermarsh reviewed on 2023-03-30 15:27_

---

_@konstin reviewed on 2023-03-30 15:29_

---

_Review comment by @konstin on `crates/ruff/src/autofix/helpers.rs`:508 on 2023-03-30 15:29_

yes, this:

```python
@cache
def foo():
    ...
```

quck fix -> enter -> enter:

```python
from functools import cache


@cache
def foo():
    ...
```

---

_Merged by @charliermarsh on 2023-03-30 15:33_

---

_Closed by @charliermarsh on 2023-03-30 15:33_

---

_Branch deleted on 2023-03-30 15:33_

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/helpers.rs`:508 on 2023-03-30 15:33_

Yeah but in that case, the user has already written `@cache`.

Given:

```py
@functools.cache
def f(x):
    return x
```

Then the same action yields:

```py
import functools

@functools.cache
def f(x):
    return x
```

Here, we don't know which the user prefers ahead-of-time.

---

_@charliermarsh reviewed on 2023-03-30 15:33_

---
