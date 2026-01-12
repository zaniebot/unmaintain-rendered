```yaml
number: 548
title: Support type annotations that use legacy typing aliases for generic classes
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - generics
assignees: []
created_at: 2025-05-30T11:09:27Z
updated_at: 2025-06-03T11:09:53Z
url: https://github.com/astral-sh/ty/issues/548
synced_at: 2026-01-12T15:54:23Z
```

# Support type annotations that use legacy typing aliases for generic classes

---

_@AlexWaygood_

ty currently only has partial support for legacy aliases such as `Dict`, `List`, `FrozenSet`, etc. For example, these two `reveal_type` calls should both reveal `dict[str, int]`, but the second one does not:

```py
from typing import Dict

def f(x: dict[str, int], y: Dict[str, int]):
    reveal_type(x)  # revealed: dict[str, int]
    reveal_type(y)  # revealed: dict[Unknown, Unknown]
```

We now support generics, so this should be fairly easy to fix. We need to resolve the TODOs in this area of the code: https://github.com/astral-sh/ruff/blob/ad2f667ee4323dd0c12224338734d722d13d28b4/crates/ty_python_semantic/src/types/infer.rs#L8753-L8789

---

_Label `help wanted` added by @AlexWaygood on 2025-05-30 11:09_

---

_Label `generics` added by @AlexWaygood on 2025-05-30 11:09_

---

_Comment by @lipefree on 2025-05-30 12:06_

I can try to take a look at this !

---

_Assigned to @lipefree by @AlexWaygood on 2025-05-30 12:07_

---

_Comment by @lipefree on 2025-05-30 13:27_

If someone can help me understand the following. I have managed to implement the feature for `List[T]` but still struggles when I have more than one type for example `Dict[str, int]`. I have tried to do

```rust
            KnownInstanceType::Dict => {
                let ty = self.infer_type_expression(arguments_slice);
                KnownClass::Dict.to_specialized_instance(db, [ty, ty]) // reveal: Dict[Unknown, Unkown]
            }
            KnownInstanceType::List => {
                let ty = self.infer_type_expression(arguments_slice);
                KnownClass::List.to_specialized_instance(db, [ty]) // reveal: List[str] ✓
            }
```
But it doesn't seem to work and I don't know how to decompose `ty` into 2 part to pass to the `to_specialized_instance` when it requires 2 types.

---

_Comment by @dcreager on 2025-05-30 14:12_

If there are two arguments provided, `arguments_slice` will be a tuple expression, and you'll want to call `infer_type_expression` on each subexpression of that tuple. There's an example of doing that [here](https://github.com/astral-sh/ruff/blob/d65bd69963e8b6ec05e465b1ecbdd391883645ef/crates/ty_python_semantic/src/types/infer.rs#L7279-L7291). [This line](https://github.com/astral-sh/ruff/blob/d65bd69963e8b6ec05e465b1ecbdd391883645ef/crates/ty_python_semantic/src/types/infer.rs#L7282) in particular gives you an `Iterator` of the inferred types of each element of the tuple, which you can pass in directly as the second parameter to `to_specialized_instance`. That method will automatically detect and raise a diagnostic if the tuple has the wrong number of elements. (We might consider a more specific diagnostic, but that can be a follow-on!)

---

_Comment by @lipefree on 2025-05-30 16:21_

Thank you very much for the detailed explanation @dcreager ! 

---

_Comment by @lipefree on 2025-05-30 23:04_

I am stuck for a few hours now and can't seem to debug it. So I have implemented the following (example for `Dict`)

```rust
KnownInstanceType::Dict => {
    if let ast::Expr::Tuple(tuple) = arguments_slice {
        let ty_iter = tuple.elts.iter().map(|elt| self.infer_type_expression(elt));
        KnownClass::Dict.to_specialized_instance(db, ty_iter)
    } else {
        KnownClass::Dict.to_instance(db)
    }
}
```
And the feature even work when I run this code on some python script, it successfully reveal `dict[str, int]` !

However when running all the tests, I get the following : 

```
test linter_af_no_panic ... FAILED
test corpus_no_panic ... ok
test parser_no_panic ... ok
test linter_gz_no_panic ... FAILED

failures:

---- linter_af_no_panic stdout ----
[...]
checking crates/ruff_linter/resources/test/fixtures/fastapi/FAST001.py

thread 'linter_af_no_panic' panicked at crates/ty_python_semantic/src/semantic_model.rs:58:44:
expression should belong to this TypeInference region and TypeInferenceBuilder should have inferred a type for it
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Check failed for "crates/ruff_linter/resources/test/fixtures/fastapi/FAST001.py". Consider fixing it or adding it to KNOWN_FAILURES

---- linter_gz_no_panic stdout ----
[...]
checking crates/ruff_linter/resources/test/fixtures/pyflakes/F401_34.py

thread 'linter_gz_no_panic' panicked at /Users/lipefree/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/function/execute.rs:209:25:
infer_definition_types(Id(115b1)): execute: too many cycle iterations

thread 'linter_gz_no_panic' panicked at /Users/lipefree/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/function/execute.rs:209:25:
infer_definition_types(Id(115e6)): execute: too many cycle iterations
checking crates/ruff_linter/resources/test/fixtures/pyflakes/F401_4.py
[...]

thread 'linter_gz_no_panic' panicked at crates/ty_python_semantic/src/semantic_model.rs:58:44:
expression should belong to this TypeInference region and TypeInferenceBuilder should have inferred a type for it
Check failed for "crates/ruff_linter/resources/test/fixtures/refurb/FURB131.py". Consider fixing it or adding it to KNOWN_FAILURES

failures:
    linter_af_no_panic
    linter_gz_no_panic
``` 

I don't even know how I get linter errors when only touching at one file in `ty_python_semantic`. :(

---

_Comment by @carljm on 2025-05-31 00:22_

@lipefree Sorry, this is a confusing error. Our tests run CI against various files in the Ruff codebase, including linter test files, just to verify that we can check them without crashing. So it's not that you broke the linter, it's that there was an error in ty checking those particular linter test files.

And in these tests we do one other thing, which is that we verify that every AST node has a type stored for it (so that e.g. you could see a type when you hover over it in an editor). This is something that ty normally doesn't verify, which explains why it works for you in normal type checking but fails in these tests.

The fix is that you need to make sure a type is stored for every AST node. If you call `self.infer_type_expression(...)` on a node, it will both infer and store a type for that node. But in this case I think you also need to store a type for the overall `arguments_slice` node, using `self.store_expression_type(...)`. If you grep you can probably find other examples of this, even in similar cases with an `arguments_slice` in a subscript.

Hope that helps unblock you!

---

_Closed by @AlexWaygood on 2025-06-03 11:09_

---
