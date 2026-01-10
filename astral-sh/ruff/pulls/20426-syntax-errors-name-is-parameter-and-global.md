```yaml
number: 20426
title: "[syntax-errors] Name is parameter and global"
type: pull_request
state: merged
author: 11happy
labels:
  - rule
assignees: []
merged: true
base: main
head: name_is_param_and_global
created_at: 2025-09-15T22:47:58Z
updated_at: 2025-10-21T17:02:29Z
url: https://github.com/astral-sh/ruff/pull/20426
synced_at: 2026-01-10T17:34:34Z
```

# [syntax-errors] Name is parameter and global

---

_Pull request opened by @11happy on 2025-09-15 22:47_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR implements a new semantic syntax error where name is parameter & global.

## Test Plan

<!-- How was it tested? -->
I have written inline test as directed in #17412 


---

_Review requested from @MichaReiser by @11happy on 2025-09-15 22:47_

---

_Review requested from @dhruvmanila by @11happy on 2025-09-15 22:47_

---

_Comment by @github-actions[bot] on 2025-09-15 23:06_

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

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:93 on 2025-09-16 20:33_

This kind of traversal isn't fully correct. It will only catch top-level `global` statements like the test case you included, but there are other ways of nesting `global` statements within other statements that are still syntax errors. A couple of simple examples are `if` and `try` statements:

```pycon
>>> def f(a):
...     if True:
...         global a
...
  File "<python-input-3>", line 3
    global a
    ^^^^^^^^
SyntaxError: name 'a' is parameter and global
>>> def f(a):
...     try:
...         global a
...     except:
...         pass
...
  File "<python-input-8>", line 3
    global a
    ^^^^^^^^
SyntaxError: name 'a' is parameter and global
```

I added these examples on your branch locally, and they were not identified as errors.

The way we handle this for `LoadBeforeGlobalDeclaration` is by using the `SemanticSyntaxContext::global` method, which checks whether a given binding has been declared `global`:

https://github.com/astral-sh/ruff/blob/c3f2187fdae2f706488ec193db395800ec7a9b82/crates/ruff_python_parser/src/semantic_errors.rs#L717-L720

Unfortunately that doesn't seem to work here either because we're not yet inside the function scope when traversing the function definition, and the `global` method checks the current scope in Ruff's semantic model.

I think the approach I would try would be to perform this check when visiting a `Stmt::Global` instead of when visiting the `Stmt::FunctionDef` and check if any of the names in the `global` statement shadows a parameter. It looks like we track this kind of information in the [`BindingKind::Argument`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_semantic/src/binding.rs#L532-L537) variant in Ruff and possibly [`Scope::shadowed_bindings`](https://github.com/astral-sh/ruff/blob/c3f2187fdae2f706488ec193db395800ec7a9b82/crates/ruff_python_semantic/src/scope.rs#L38). Note that the shadowed check may be necessary because cases like this still emit the `parameter and global` syntax error (this would be a good test case to have too):

```pycon
>>> def f(a):
...     a = 1
...     global a
...
  File "<python-input-9>", line 3
    global a
    ^^^^^^^^
SyntaxError: name 'a' is parameter and global
```

I think we can do something similar with ty's [`DefinitionKind::Parameter`](https://github.com/astral-sh/ruff/blob/c3f2187fdae2f706488ec193db395800ec7a9b82/crates/ty_python_semantic/src/semantic_index/definition.rs#L671), but I'm less sure about that.

If we can't do something like that, we'd need a `Visitor` to traverse the full function definition looking for `global` statements. However, that could be quite expensive, so the approach I outlined above would be preferable.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1393 on 2025-09-16 20:34_

I'd prefer not to reformat unrelated sections of code, as I mentioned on your other PR

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1542 on 2025-09-16 20:34_

We'll need some docs here too

---

_@ntBre requested changes on 2025-09-16 20:34_

---

_Review requested from @carljm by @11happy on 2025-09-29 17:09_

---

_Review requested from @AlexWaygood by @11happy on 2025-09-29 17:09_

---

_Review requested from @sharkdp by @11happy on 2025-09-29 17:09_

---

_Review requested from @dcreager by @11happy on 2025-09-29 17:09_

---

_@11happy reviewed on 2025-09-29 17:14_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:1393 on 2025-09-29 17:14_

sure , actually I am not changing these , these are automatically added by pre-commit jobs

---

_@11happy reviewed on 2025-09-29 17:15_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:1542 on 2025-09-29 17:15_

sure will add

---

_Comment by @github-actions[bot] on 2025-09-29 17:15_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-29 17:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@11happy reviewed on 2025-09-29 17:16_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:93 on 2025-09-29 17:16_

Hii @ntBre , I have tried implementation based on above pointers,  can you please take a look whenever you get a chance.

Thank you : )

---

_Review request for @dcreager removed by @MichaReiser on 2025-09-30 07:22_

---

_Review request for @carljm removed by @MichaReiser on 2025-09-30 07:22_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-09-30 07:22_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-09-30 07:22_

---

_@11happy reviewed on 2025-09-30 17:13_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:1542 on 2025-09-30 17:13_

done, I have added docs

---

_@11happy reviewed on 2025-09-30 17:14_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:93 on 2025-09-30 17:14_

Have fixed the CI errors. 
: )

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/syntax_errors/global_parameter.py`:10 on 2025-09-30 21:19_

This one is actually not an error in the REPL:

```pycon
>>> def h(a):
...     def inner():
...         global a
...
>>> h
<function h at 0x7f684173fd80>
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/global_parameter.rs`:1 on 2025-09-30 21:26_

We don't need to define a new Ruff _rule_ in this case, we can just emit this as a syntax error. The only reason we've had rules/`Violation`s in your previous PRs is that those syntax errors were previously implemented as lint rules. New syntax errors that don't correspond to existing lint rules can just be standalone syntax errors like the ones here:

https://github.com/astral-sh/ruff/blob/cacfb09a59eef3d7af8a6067d981c8e85ae5a104/crates/ruff_linter/src/checkers/ast/mod.rs#L713-L730

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:820 on 2025-09-30 21:33_

nit: I think a name like `is_parameter` or `is_bound_parameter` would be a bit better.

More importantly, and related to my comment on the test case above, I don't think this is quite right. We need to limit our parameter check to the nearest-enclosing function scope. In other words, we need to search through `self.semantic.current_scopes` more like we do in some of the other context methods:

https://github.com/astral-sh/ruff/blob/bedfc6fd063e08f4d1e76ae8a780162404079298/crates/ruff_linter/src/checkers/ast/mod.rs#L737-L749

until we find the first function scope, and then only check for shadowed bindings within that scope. We can also return early if we find a class scope, for example, because this is also valid, like the nested function case I showed above:

```pycon
>>> def h(a):
...     class C:
...         global a
...
>>>
```

I think the checks on the binding below look roughly correct, but we need to be more careful about identifying the scope holding the relevant binding.

---

_Review comment by @ntBre on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2793 on 2025-09-30 21:34_

Ideally we would implement this for ty too, but we can leave it for a follow-up if it looks complicated or if you aren't interested. That's totally fine :)

---

_@ntBre reviewed on 2025-09-30 21:35_

Thank you, this is looking good. We just need to be a bit more careful with our scope handling and revert the unnecessary `Violation` changes.

---

_@11happy reviewed on 2025-10-02 15:25_

---

_Review comment by @11happy on `crates/ruff_linter/resources/test/fixtures/syntax_errors/global_parameter.py`:10 on 2025-10-02 15:25_

yeah, now its working fine.

---

_@11happy reviewed on 2025-10-02 15:25_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/ruff/rules/global_parameter.rs`:1 on 2025-10-02 15:25_

done

---

_@11happy reviewed on 2025-10-02 15:25_

---

_Review comment by @11happy on `crates/ruff_linter/src/checkers/ast/mod.rs`:820 on 2025-10-02 15:25_

done

---

_@11happy reviewed on 2025-10-02 15:26_

---

_Review comment by @11happy on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2793 on 2025-10-02 15:26_

Sure, I am interested to implement this for ty , I would prefer to keep it as a follow-up if that sounds fine ?.

Thank you : )

---

_Review comment by @ntBre on `crates/ruff_python_parser/tests/fixtures.rs`:579 on 2025-10-06 18:55_

nit:


```suggestion

    fn is_bound_parameter(&self, _name: &str) -> bool {
```

---

_Review comment by @ntBre on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2792 on 2025-10-06 18:57_

```suggestion

    fn is_bound_parameter(&self, _name: &str) -> bool {
```

---

_Review comment by @ntBre on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2793 on 2025-10-06 18:57_

That sounds fine!

---

_@ntBre reviewed on 2025-10-06 19:12_

Thank you, this is looking good for the tests currently implemented. I think we should a few more test cases, for example:

```python
def f(a):
    a = 1
    global a  # error
```

This tests the shadowing code. I'm actually not sure I was correct to recommend checking `shadowed_bindings`. Rereading the docstring, it sounds like this only catches shadowing from different scopes. We might actually need to check for `rebinding_scopes`. In any case, a test like the one above, and possibly one with multiple rebindings, would be a good idea:

```python
def f(a):
    a = 1
    a = 2
    global a  # error
```

I see that `is_bound_parameter` also has special handling for classes, so it seems worth including a test case with an intervening class scope, probably more than one case showing some of the interleavings like function-class-function, class-function-class, etc, or at least the interesting ones. I think something like this would be interesting at a minimum:

```python
def f(a):
    class Inner:
        global a
```

I also had a couple of minor nits about spaces between functions.

---

_Comment by @11happy on 2025-10-11 12:10_

So the shadowed_bindings in `SemanticModel` and `Scope` are different, respectively. As per the example given in the docstring of `Scope`
```
/// A map from binding ID to binding ID that it shadows.
///
/// For example:
/// ```python
/// def f():
///     x = 1
///     x = 2
/// ```
///
/// In this case, the binding created by `x = 2` shadows the binding created by `x = 1`.
shadowed_bindings: FxHashMap<BindingId, BindingId>,
```
We can utilize this to trace back whether a global declaration is originally an argument or not — and I’ve done that accordingly. I also think we don’t really need to interact with rebindings_scope here. I manually checked with an example, and it was coming out to be empty in the following case:
```
def f(a):
    a = 1
    a = 2
    global a
```
Also, while trying to solve this issue, I read a lot about bindings, shadowed bindings, etc., and I’m genuinely thankful for the opportunity to work on this. The tests are working as expected now

Thank you : )

---

_@11happy reviewed on 2025-10-11 12:14_

---

_Review comment by @11happy on `crates/ruff_python_parser/tests/fixtures.rs`:579 on 2025-10-11 12:14_

done

---

_Review comment by @11happy on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2792 on 2025-10-11 12:14_

done

---

_@11happy reviewed on 2025-10-11 12:14_

---

_Comment by @11happy on 2025-10-12 09:13_

is the `CI/fuzz parser` error related to this PR changes ?


---

_@ntBre reviewed on 2025-10-13 19:23_

---

_Review comment by @ntBre on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__semantic_syntax_error_GlobalParameter_global_parameter_3.10.snap`:64 on 2025-10-13 19:23_

This is what the fuzzer seeds that are failing look like. This is not supposed to be an error:

```pycon
>>> def f(a):
...     class Inner:
...         global a
...
>>> f
<function f at 0x7fbec085be20>
```


---

_@11happy reviewed on 2025-10-14 16:52_

---

_Review comment by @11happy on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__semantic_syntax_error_GlobalParameter_global_parameter_3.10.snap`:64 on 2025-10-14 16:52_

done , have fixed it & fixed the merge conflict as well (rebased)
Thank you : ) 

---

_@11happy reviewed on 2025-10-14 16:53_

---

_Review comment by @11happy on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__semantic_syntax_error_GlobalParameter_global_parameter_3.10.snap`:64 on 2025-10-14 16:53_

Currently we are doing an early return for class scope , please let me know if there are any more edge cases you would like me to test , would be happy to add them.

Thank you

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:850 on 2025-10-15 15:45_

Ah, I overlooked this initially, but the `ScopeKind`s here actually contain these nodes' definitions. We should be able to extract the parameters from that and avoid this binding stuff entirely. I always get a little nervous when I see a `loop`, so I looked a bit harder at this today.

```rust
    fn is_bound_parameter(&self, name: &str) -> bool {
        for scope in self.semantic.current_scopes() {
            match scope.kind {
                ScopeKind::Class(_) => return false,
                ScopeKind::Function(ast::StmtFunctionDef { parameters, .. })
                | ScopeKind::Lambda(ast::ExprLambda {
                    parameters: Some(parameters),
                    ..
                }) => return parameters.includes(name),
                ScopeKind::Lambda(_)
                | ScopeKind::Generator { .. }
                | ScopeKind::Module
                | ScopeKind::Type
                | ScopeKind::DunderClassCell => {}
            }
        }

        false
    }
```

I believe it's actually impossible for a lambda to contain a `global` since it can only contain a single expression and not a statement, so we could probably eliminate the lambda branch here too.

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:1043 on 2025-10-15 15:50_

We shouldn't do this in this PR, but I think it would be nice to move these test cases out to separate test files. It's getting a bit unwieldy. Feel free to follow up on that if you're interested :)

---

_@ntBre reviewed on 2025-10-15 15:52_

Thanks, this is looking good. I had one idea to simplify `is_bound_parameter` and a suggestion for a follow-up.

---

_@11happy reviewed on 2025-10-16 16:49_

---

_Review comment by @11happy on `crates/ruff_linter/src/checkers/ast/mod.rs`:850 on 2025-10-16 16:49_

done

---

_@11happy reviewed on 2025-10-16 16:50_

---

_Review comment by @11happy on `crates/ruff_linter/src/linter.rs`:1043 on 2025-10-16 16:50_

Sure, would open a follow up PR.

Thank you

---

_@11happy reviewed on 2025-10-16 16:53_

---

_Review comment by @11happy on `crates/ruff_linter/src/checkers/ast/mod.rs`:850 on 2025-10-16 16:53_

Now that I’m looking at this and the previous iterations, I feel like if we’re starting to make the code more complex, something’s probably off with the approach. From now on, I’ll be more careful and focus on keeping things simpler.

Thank you : )

---

_@11happy reviewed on 2025-10-16 18:37_

---

_Review comment by @11happy on `crates/ruff_linter/src/linter.rs`:1043 on 2025-10-16 18:37_

I have created a PR here https://github.com/astral-sh/ruff/pull/20926

---

_@ntBre approved on 2025-10-21 16:44_

Thank you!

---

_Renamed from "[Syntax-errors] : Implement semantic syntax error where name is parameter and global" to "[syntax-errors] Name is parameter and global" by @ntBre on 2025-10-21 16:45_

---

_Label `rule` added by @ntBre on 2025-10-21 16:46_

---

_Merged by @ntBre on 2025-10-21 16:51_

---

_Closed by @ntBre on 2025-10-21 16:51_

---
