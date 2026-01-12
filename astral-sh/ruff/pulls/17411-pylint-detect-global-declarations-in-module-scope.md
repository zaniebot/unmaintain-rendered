```yaml
number: 17411
title: "[`pylint`] Detect `global` declarations in module scope (`PLE0118`)"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
assignees: []
merged: true
base: main
head: brent/ple0118-in-module-scope
created_at: 2025-04-15T16:43:06Z
updated_at: 2025-04-25T12:37:18Z
url: https://github.com/astral-sh/ruff/pull/17411
synced_at: 2026-01-12T15:56:02Z
```

# [`pylint`] Detect `global` declarations in module scope (`PLE0118`)

---

_@ntBre_

Summary
--

While going through the syntax errors in [this comment], I was surprised to see the error `name 'x' is assigned to before global declaration`, which corresponds to [load-before-global-declaration (PLE0118)] and has also been reimplemented as a syntax error (#17135). However, it looks like neither of the implementations consider `global` declarations in the top-level module scope, which is a syntax error in CPython:

```python
# try.py
x = None
global x
```

```shell
> python -m compileall -f try.py
Compiling 'try.py'...
***   File "try.py", line 2
    global x
    ^^^^^^^^
SyntaxError: name 'x' is assigned to before global declaration
```

I'm not sure this is the best or most elegant solution, but it was a quick fix that passed all of our tests.

Test Plan
--

New PLE0118 test case.

[this comment]: https://github.com/astral-sh/ruff/issues/7633#issuecomment-1740424031
[load-before-global-declaration (PLE0118)]: https://docs.astral.sh/ruff/rules/load-before-global-declaration/#load-before-global-declaration-ple0118

---

_Label `bug` added by @ntBre on 2025-04-15 16:43_

---

_Comment by @github-actions[bot] on 2025-04-15 16:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/model.rs`:1529 on 2025-04-15 17:32_

This is the part I don't like. Without it, one F821 test fails:

https://github.com/astral-sh/ruff/blob/ff420082e4517963ca7740873b7068c1619daf69/crates/ruff_linter/src/rules/pyflakes/mod.rs#L1035-L1046

because `set_globals` adds a binding for `x`.

---

_@ntBre reviewed on 2025-04-15 17:32_

---

_Marked ready for review by @ntBre on 2025-04-15 17:34_

---

_@MichaReiser reviewed on 2025-04-23 06:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:1529 on 2025-04-23 06:44_

I don't think I fully understand this change. Isn't `self.at_top_level` always `true` when called from `visit_module` and doesn't the gating here make the `set_globals` call in `visit_module` a no-op?

---

_@ntBre reviewed on 2025-04-23 13:38_

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/model.rs`:1529 on 2025-04-23 13:38_

It looked like you were exactly right when I looked back at this diff, but there's one extra line in this function outside of the diff window:

```rust
self.scopes[self.scope_id].set_globals_id(self.globals.push(globals));
```

We still want this to run to record the `global` but not add a binding for it at the top level. It would make some sense only to inline this rather than call `set_globals` but some of these `SemanticModel` fields are private, so I think that's why I added the conditional instead.

---

_Comment by @MichaReiser on 2025-04-23 20:28_

Can you tell me a bit more about the motivation for adding this error? I'm not quiet convinced that we should emit the extra error at the module level (especially considering that it results in a 1% perf regression)

What's not quiet clear to me is how many errors will emit in the case where there's an assignment to a variable before its global declaration at the module level. What I understand from the PR summary is that will emit two errors:

1. That the globals statement is invalid on the module level
2. That `x` is used before the globals declaration

If that's the case, then I'd prefer not to make this change because the only relevant error for users to fix is to *not* use the globals statement and everything will then work as expected (at least for assignments, uses are a different story).



---

_Comment by @ntBre on 2025-04-23 21:17_

> Can you tell me a bit more about the motivation for adding this error? I'm not quiet convinced that we should emit the extra error at the module level (especially considering that it results in a 1% perf regression)

The main motivation was just that it came up in the fuzzing results from #7633, and I noticed that we didn't raise any error for it.

> What's not quiet clear to me is how many errors will emit in the case where there's an assignment to a variable before its global declaration at the module level. What I understand from the PR summary is that will emit two errors:
> 
> 1. That the globals statement is invalid on the module level
> 2. That `x` is used before the globals declaration
> 
> If that's the case, then I'd prefer not to make this change because the only relevant error for users to fix is to _not_ use the globals statement and everything will then work as expected (at least for assignments, uses are a different story).

I think the summary (and title) was a bit poorly worded. `global` is valid at the module level as far as CPython is concerned, only accessing the variable before the `global` statement is an error. This snippet runs fine in Python:

```python
global x
x = 1
```

What I was trying to say in the title/description was more like "extend `PLE0118` to detect load-before-global-declaration in the module scope," but I see now how it sounds very different. `PLE0118` currently only applies in function and class scopes but should also apply in module scope to match CPython.

Ideally this would have been a rule-specific change, but instead it looked like I needed to modify the `Checker`/`SemanticModel` like I did here.

I don't feel strongly about this, though, especially with the perf hit, and am happy to close the PR. I only opened it for completeness of handling #7633. I think actual code like this would be pretty rare.

---

_Comment by @MichaReiser on 2025-04-24 06:35_

Thanks. So there's nothing special about the module level. It's just that Ruff didn't emit the diagnostic 

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:1506 on 2025-04-24 06:36_

Nit: You could reduce the diff size by
* Moving `self.scopes[self.scope_id].set_globals_id(self.globals.push(globals));` to the top
* Use an early return if `self.at_top_level()` is true



---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:1508 on 2025-04-24 06:40_

Could you maybe add an example here of what problem would arise if we did run this code at the global level too? It's not entirely clear to me and I'm wondering if *not running it* creates different problems at the root level (you could try to comment out this code and see what tests fail for the non-global scope and then decide if this is or isn't a problem for the global scope)

---

_@MichaReiser approved on 2025-04-24 06:40_

---

_@ntBre reviewed on 2025-04-24 15:45_

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/model.rs`:1506 on 2025-04-24 15:45_

I tried that now, and it causes several issues because we're moving `globals` here: `self.globals.push(globals)`. Even saving the index returned by `push` causes problems because then we're iterating over `self.globals[globals_id]`, while also trying to mutate `self.global_scope_mut()` at the end of the loop.

It was a good idea I hadn't considered, but I don't think it works easily without cloning or swapping `globals` somehow.

---

_@ntBre reviewed on 2025-04-24 16:05_

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/model.rs`:1508 on 2025-04-24 16:05_

Ah I did try deleting this code entirely before because that actually seemed more natural to me, but I didn't write down the results. This is the tricky F821 (`F821_6.py`) case that fails if we comment out the binding loop entirely:

```python
"""Test: annotated global."""


n: int


def f():
    print(n)  # F821 false positive here


def g():
    global n
    n = 1


g()
f()
```

This shorter case also gets a false positive with the whole loop commented out:

```python
def f(): global foo; import foo
def g(): foo.is_used()  # false positive here
```

I don't think we can run into something similar if the `global` is in the module scope, but this was all quite tricky, so I'm glad to get your eyes on these examples too.

If we run the binding loop in the global scope too, this case, also for F821, fails:

```python
global x
def foo():
    print(x)  # F821 false negative
```

The second and third cases here look like nice additions to the docs.

---

_@ntBre reviewed on 2025-04-24 18:57_

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/model.rs`:1508 on 2025-04-24 18:57_

Actually, the more I stare at these examples, the more I think I might have been right to consider deleting this code entirely. It's really the assignment and the import that should be adding the bindings, not the `global` statements. We can just coincidentally get the right answer by letting `global` handle it, but it's more obvious that a real binding is needed in the module scope.

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/model.rs`:1508 on 2025-04-24 20:11_

I don't think this is easily actionable though. I poked around a bit in `Checker::add_binding` and my attempts at checking for `global`s didn't work out. I've added the examples for now.

---

_@ntBre reviewed on 2025-04-24 20:11_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:1529 on 2025-04-25 06:55_

Thanks, the comment clarifies it a lot! 

---

_@MichaReiser reviewed on 2025-04-25 06:55_

---

_Merged by @ntBre on 2025-04-25 12:37_

---

_Closed by @ntBre on 2025-04-25 12:37_

---

_Branch deleted on 2025-04-25 12:37_

---
