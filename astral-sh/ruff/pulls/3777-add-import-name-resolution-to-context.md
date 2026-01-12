```yaml
number: 3777
title: "Add import name resolution to `Context`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/resolve-binding
created_at: 2023-03-28T16:49:52Z
updated_at: 2023-03-30T06:32:04Z
url: https://github.com/astral-sh/ruff/pull/3777
synced_at: 2026-01-12T04:39:45Z
```

# Add import name resolution to `Context`

---

_Pull request opened by @charliermarsh on 2023-03-28 16:49_

## Summary

Right now, we make heavy use of `resolve_call_path`, which takes an expression as input and resolves it to a fully qualified import name, if possible.

For example, given:
```python
from sys import version_info as python_version
print(python_version)
```

...then `resolve_call_path(${python_version})` would resolve to `sys.version_info`.

This PR implements the reverse lookup: given an import name, can we find a binding in the current scope that maps to that import name? E.g., if we want to insert a reference to `sys.version`, can we write a function that returns `python_version`?

This method is implemented here as `resolve_binding`. It builds on logic that already existed in `crates/ruff/src/rules/pylint/rules/sys_exit_alias.rs`, but (1) generalizes it such that any module can tap into it, and (2) fixes at least one bug. I've also modified an additional rule (`crates/ruff/src/rules/pyupgrade/rules/lru_cache_with_maxsize_none.rs`) to make use of it.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-28 16:49_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pylint/sys_exit_alias_3.py`:16 on 2023-03-28 16:50_

The existing version of `resolve_binding` in `sys_exit_alias` didn't take into account that bindings might be overridden. We now avoid trying to fix this (since the `exit` in `from sys import exit` (out-of-view, on Line 1) is overridden here).

---

_@charliermarsh reviewed on 2023-03-28 16:50_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/context.rs`:237 on 2023-03-28 16:51_

This whole thing has the potential to be pretty slow.

We have to iterate over all bindings in scope (to find candidates), then iterate over the scopes we passed over to ensure that the candidates weren't overridden at a lower level. I'm sure we could make this much faster, but it'd require different bookkeeping.

---

_@charliermarsh reviewed on 2023-03-28 16:51_

---

_@charliermarsh reviewed on 2023-03-28 16:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/context.rs`:231 on 2023-03-28 16:54_

We could track these as we go for the common case (e.g., for each scope we pass over, check if `sys` is overridden, and just make a note of it to avoid this nested loop). However, that wouldn't catch aliased imports (we can't know which aliased imports might be overridden in advance, since we don't know how they're aliased ahead-of-time!). I'd guess aliased imports are much less common, so maybe that's a trivial and helpful optimization?

---

_Comment by @github-actions[bot] on 2023-03-28 17:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.50ms     2.4 MB/sec    1.01     17.2±0.45ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.10ms     3.9 MB/sec    1.01      4.3±0.10ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   574.9±20.78µs     5.1 MB/sec    1.00   570.8±19.71µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.4±0.25ms     3.4 MB/sec    1.00      7.3±0.21ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.15ms     4.7 MB/sec    1.02      8.9±0.22ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1945.3±49.34µs     8.6 MB/sec    1.01  1960.5±58.26µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   237.5±12.68µs    12.4 MB/sec    1.02   241.1±13.08µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.10ms     6.3 MB/sec    1.01      4.1±0.12ms     6.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.1±0.36ms     2.5 MB/sec    1.00     16.2±0.37ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.07ms     4.0 MB/sec    1.00      4.1±0.07ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   504.0±11.78µs     5.9 MB/sec    1.00    497.8±8.55µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.14ms     3.8 MB/sec    1.00      6.8±0.12ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.12ms     4.9 MB/sec    1.00      8.3±0.12ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1799.6±27.67µs     9.3 MB/sec    1.01  1825.3±29.33µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    195.7±5.14µs    15.1 MB/sec    1.00    194.3±5.40µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.08ms     6.6 MB/sec    1.00      3.8±0.08ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @JonathanPlasse on 2023-03-28 17:48_

Glad to see my code reused and improved :smiley: 

---

_Comment by @charliermarsh on 2023-03-28 17:58_

👏 👏 👏 


---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/context.rs`:141 on 2023-03-29 08:16_

```suggestion
    /// Resolves the [`Expr`] to a fully-qualified symbol-name, if `value` resolves to an imported symbol.
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/context.rs`:220 on 2023-03-29 08:17_

Nit: change the visibility to `pub(crate)`

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/context.rs`:220 on 2023-03-29 09:44_

The way I think about this method is that it does two things:
* Resolves the local symbol for a module with the given full qualified name or its parent module.
* Return the full qualified name to access that module member in the local context. 

I recommend renaming the method because it doesn't return a binding nor is it a generic implementation that returns any binding. I'm struggling with naming because the method does two things but I was thinking about `resolve_qualified_import_name`

I wonder how an API would look like that splits the responsibility, but that's probably too much work just now without building more of the semantic model infrastructure. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/context.rs`:225 on 2023-03-29 09:45_

Nit: You can directly iterate over the bindings
```suggestion
            scope.binding().iter().find_map(|binding| match binding.kind {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/context.rs`:231 on 2023-03-29 09:51_

I think what we have here is "fine" for now but we should invest into building a full semantic model as part of the parsing or syntax traversal, and then use it to answer these kind of questions.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/context.rs`:220 on 2023-03-29 09:53_

Nit: You could avoid allocating in some cases by returning `Option<Cow<str>>`, but I don't know if its worth here or if it would be better to return an `enum` instead that then has a method to get the full qualified name of the local symbol referencing the import. 

---

_@MichaReiser approved on 2023-03-29 09:53_

This looks good to me to unblock some work, but I think we want to take a more holistic approach to solve semantic analysis / symbol table in the future to better balance performance with the full power of a symbol table and to enable cross file analysis. 

I think our ultimate goal should be to have a semantic model that exposes rich information about symbols and lets us:

* resolve the symbol for a reference
* get all references of a symbol
* get all symbols for a scope
* get all declarations/definitions of a symbol. 
* get all members of a symbol (class members)
* get all exports/imports of a module (similar to getting all members of a symbol)

Similar to what the semantic model of [oxc](https://github.com/Boshen/oxc/tree/main/crates/oxc_semantic), [rome](https://github.com/rome/tools/blob/main/crates/rome_js_semantic/src/semantic_model/model.rs), or [TypeScript](https://basarat.gitbook.io/typescript/overview/binder) offer. 

Our own parser would allow us to build the semantic model (or the scope table at least) while parsing or building the AST. But it may still be required to split ruff into multiple passes:

* syntax lints
* semantic lints
* project/solution lints

## Terminology 

I noticed is that we have a different understanding of the terms *symbol* and *binding* that I think is worth clarifying. Let me explain my understanding and then try to align on a shared terminology. 

I'll use the following code for  my explanation:

```python
import sys

class Test:
	class_field = 1
	
	def __init__(self):
		self.a = 10

	def class_method():
		pass

	def method(self):
		pass

def f():
	pass

a = 10
a = a + 2

test = new Test()
test.a = 2
```

### Symbols

A symbol is the unique combination of a "name" and its scope. For example, `a` in `a + 2` references the variable `a` from the local scope, whereas `test.a` references the class instance symbol `a` of `Test`. 

Every symbol has a name that uniquely identifies it in the current scope: Examples are `a`, `test`, `Test`, `sys`. 

A symbol can have a full qualified name. For example: the full qualified name of the member `exit` on `sys` is `sys.exit`.

### Definition/Declaration
A declaration is the expression or statement that *creates* the new symbol. `class Test` is the declaration for the `Test` symbol, or `a = 10` declares the variable `a`. 

A symbol can have zero to 1 or many definition/declarations. 

* 0: global or undeclared symbol
* 1: Most common: Symbol with a declaration in source code or a definition in a typing declaration file.
* Many: 
	* symbol defined in a typing declaration file AND declared in a python file. 

### References / Uses
Read or write access to a symbol after its declaration. This can be a function call (lookup of the function), variable or member assignments, or normal variables. 

Examples:
* `a = a + 2`: References the `a` symbol
* `new Test()`: References the `Test` and, implicitly, the `__init__` symbol
* `test.a = 3`: References the `test` and the `Test.a` symbol

It can be useful to track whether it's a read or write a reference, e.g., to recommend renaming a supposedly constant to a non-constant if there are any write references.

### Binding
The part I struggle most with explaining and have only found little reference is *Binding*. 

I think that a *binding* is the combination of a *Symbol* with its value (or metadata). In our case the *Binding* is the symbol table entry that stores the mapping from symbol to its declarations and references. 

For example, `a` is a binding. It has one declaration `a = 10`, and two references: `a`, and 
a = a + 2`

### Scope
Implements the encapsulation of symbols: symbols are only visible to child scopes and shadowing overrides variables from the outer scope. 

### Symbol Table
Stores the symbols of a program. A question here is if symbols with members (e.g. classes) have their own symbol table/ implement symbol table too, or if that's implemented on top of scopes. 

### Semantic model
Contaisn the symbol table but may also give access to type information. 

Stores the symbols for a program

---

_Comment by @charliermarsh on 2023-03-29 13:26_

This is extremely helpful. I'd love to keep pushing on improving the semantic model. (A good example of something that we couldn't do right now is: "rename a variable" (even within a single file).)

Let me throw out a few more examples to make sure we're aligned on terminology:

```py
from sys import exit
from sys import exit as foo
```

This would introduce two symbols (`exit` and `foo`). I _think_ they'd have the same "fully-qualified name"? And they are references to the same underlying "thing" in `sys`, though I'm not certain what that thing should be called. I think it's a symbol in `sys`.

```py
a = 10
a = a + 2
```

In this case, it's just _one_ symbol (`a`) even though it's rebound, right?

---

_Comment by @MichaReiser on 2023-03-29 13:44_

> This would introduce two symbols (exit and foo). I think they'd have the same "fully-qualified name"? And they are references to the same underlying "thing" in sys, though I'm not certain what that thing should be called. I think it's a symbol in sys.

This is a great question, and I don't have a particularly good answer for it. It would be interesting to look at other tools.

I understand that there are multiple symbols at play. 
* One `external` symbol `sys` for the module
* The module `sys` has one member `exit` 
* The local symbol `exit` is an alias for the `exit` member of `sys`. 
* The local symbol `foo` is an alias for the `exit` member of `sys`.

So yes, both `exit` and `foo` point to the same external symbol. But I'm not sure how to best represent this relationship. 

> In this case, it's just one symbol (a) even though it's rebound, right?

Yes, depending on the definition, `a` as two or three references: one read, and one or two writes.

---

_Comment by @charliermarsh on 2023-03-29 18:36_

In this framing, would you agree that the _key_ to the `scope.values` hash table is the "symbol", and the value is the "binding" (`Binding`)?

---

_@charliermarsh reviewed on 2023-03-29 21:35_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/context.rs`:220 on 2023-03-29 21:35_

It's not `pub(crate)` though, we need to access this from `ruff` :)

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/context.rs`:225 on 2023-03-29 21:38_

`binding` iterates over (name, `BindingId`) pairs, so we still have to do this lookup I think?

---

_@charliermarsh reviewed on 2023-03-29 21:38_

---

_Renamed from "Implement symbol-to-binding resolution for `Context`" to "Add import name resolution to `Context`" by @charliermarsh on 2023-03-29 21:42_

---

_Merged by @charliermarsh on 2023-03-29 21:47_

---

_Closed by @charliermarsh on 2023-03-29 21:47_

---

_Branch deleted on 2023-03-29 21:47_

---

_Comment by @MichaReiser on 2023-03-30 06:32_

> In this framing, would you agree that the _key_ to the `scope.values` hash table is the "symbol", and the value is the "binding" (`Binding`)?

Not sure. In my mind the binding is the association of a "name" (Symbol) to its location (if you emit bytecode) or metadata (static analyzer). But I think that's a detail and calling the metadata `Binding` is fine (especially if it has a back reference to the symbol)

---
