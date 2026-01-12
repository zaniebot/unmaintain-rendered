```yaml
number: 15565
title: "[`pyupgrade`] Add rules to use PEP 695 generics in classes and functions (`UP046`, `UP047`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/pep695-generics
created_at: 2025-01-18T04:17:41Z
updated_at: 2025-01-22T16:35:24Z
url: https://github.com/astral-sh/ruff/pull/15565
synced_at: 2026-01-12T15:55:51Z
```

# [`pyupgrade`] Add rules to use PEP 695 generics in classes and functions (`UP046`, `UP047`)

---

_@ntBre_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR extends our [PEP 695](https://peps.python.org/pep-0695) handling from the type aliases handled by `UP040` to generic function and class parameters, as suggested in the latter two examples from #4617:

```python
# Input
T = TypeVar("T", bound=float)
class A(Generic[T]):
    ...

def f(t: T):
    ...

# Output
class A[T: float]:
    ...

def f[T: float](t: T):
    ...
```

I first implemented this as part of `UP040`, but based on a brief discussion during a very helpful pairing session with @AlexWaygood, I opted to split them into rules separate from `UP040` and then also separate from each other. From a quick look, and based on [this issue](https://github.com/asottile/pyupgrade/issues/836), I'm pretty sure neither of these rules is currently in pyupgrade, so I just took the next available codes, `UP046` and `UP047`.

The last main TODO, noted in the rule file and in the fixture, is to handle generic method parameters not included in the class itself, `S` in this case:

```python
T = TypeVar("T")
S = TypeVar("S")

class Foo(Generic[T]):
    def bar(self, x: T, y: S) -> S: ...
```

but Alex mentioned that that might be okay to leave for a follow-up PR.

I also left a TODO about handling multiple subclasses instead of bailing out when more than one is present. I'm not sure how common that would be, but I can still handle it here, or follow up on that too.

I think this is unrelated to the PR, but when I ran `cargo dev generate-all`, it removed the rule code `PLW0101` from `ruff.schema.json`. It seemed unrelated, so I left that out, but I wanted to mention it just in case.

## Test Plan

New test fixture, `cargo nextest run`

Closes #4617, closes #12542 


---

_Label `rule` added by @ntBre on 2025-01-18 04:20_

---

_Comment by @github-actions[bot] on 2025-01-18 04:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `preview` added by @ntBre on 2025-01-18 04:33_

---

_Review requested from @AlexWaygood by @AlexWaygood on 2025-01-19 13:22_

---

_Comment by @ntBre on 2025-01-20 00:06_

I'm not quite sure what's going wrong with `mkdocs`. I tried pasting in the code it suggested and also adding the rule to `KNOWN_FORMATTING_VIOLATIONS`, but running `python scripts/check_docs_formatted.py` locally still reported an issue after each attempt. I'm planning to try again tomorrow morning, but otherwise this should still be ready for review.

I was also mildly surprised to see no changes in the ecosystem checks. Do those run with Python 3.12?

---

_Comment by @dhruvmanila on 2025-01-20 05:39_

> I'm not quite sure what's going wrong with `mkdocs`. I tried pasting in the code it suggested and also adding the rule to `KNOWN_FORMATTING_VIOLATIONS`, but running `python scripts/check_docs_formatted.py` locally still reported an issue after each attempt. I'm planning to try again tomorrow morning, but otherwise this should still be ready for review.

I'm not exactly sure but making the change it suggested seems to work for me 😅 

The script runs on the generated docs which means that you'll have to generate the docs every time you make a change in the Rust code. It's always preferable to include the `--generate-docs` when running this script just to make sure that it's running on the latest docs version.


> I was also mildly surprised to see no changes in the ecosystem checks. Do those run with Python 3.12?

Yes, the ecosystem checks are run on Python 3.12
https://github.com/astral-sh/ruff/blob/c6c434cbc75497dbba65caef892801759aeff6d2/.github/workflows/ci.yaml#L404-L420

https://github.com/astral-sh/ruff/blob/c6c434cbc75497dbba65caef892801759aeff6d2/.github/workflows/ci.yaml#L19-L19

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:17 on 2025-01-20 09:56_

I would avoid using AST names in user facing documentation because I doubt many users are familiar with their names. Instead, I suggest using the same terminology as PEP 695: Type parameter syntax, generic functions and classes

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695.rs`:1 on 2025-01-20 10:01_

Did you make any changes to the code in this module or did you copy it over as is?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP046.py.snap`:8 on 2025-01-20 10:04_

Should we change the diagnostic range to only the `Generic[T]` instead of the entire class header because the name or other base classes aren't a problem.

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046.py`:9 on 2025-01-20 10:07_

Can we add an example where the class has multiple base classes (I don't know if that's valid)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695.rs`:87 on 2025-01-20 10:11_

I generally prefer to implement `Display` over having `fmt_` methods. It has the advantage that you can call `to_string` if you want to format it to a `String` or that it can be formatted without allocating an extra string, by directly writing into the target string (using `f`)


```suggestion
    impl TypeVar<'_> {
        fn display<'a>(&'a self, source: &'a str) -> DisplayTypeVar<'a> {
            DisplayTypeVar { type_var: self, source }
        }
    }

    struct DisplayTypeVar<'a> {
        type_var: &'a TypeVar<'a>,
        source: &'a str
    }

    impl Display for DisplayTypeVar<'_> {
        fn fmt(...) {
            match self.type_var {
                ...
            }
        }
    }


    /// Format `self` into `s`, where `source` is the whole file, which will be sliced to recover
    /// the `TypeVarRestriction` values for generic bounds and constraints.
    fn fmt_into(&self, s: &mut String, source: &str) {
        match self.kind {
            TypeVarKind::Var => {}
            TypeVarKind::Tuple => s.push('*'),
            TypeVarKind::ParamSpec => s.push_str("**"),
        }
        s.push_str(&self.name.id);
        if let Some(restriction) = &self.restriction {
            s.push_str(": ");
            match restriction {
                TypeVarRestriction::Bound(bound) => {
                    s.push_str(&source[bound.range()]);
                }
                TypeVarRestriction::Constraint(vec) => {
                    let len = vec.len();
                    s.push('(');
                    for (i, v) in vec.iter().enumerate() {
                        s.push_str(&source[v.range()]);
                        if i < len - 1 {
                            s.push_str(", ");
                        }
                    }
                    s.push(')');
                }
            }
        }
    }
```

and similar, add a `DisplayTypeVars` wrapper. You could decide to skip the `DisplayTypeVar` struct and instead make `fmt_into` a method on `DisplayTypeVars` with the signature `fmt_type_var(f, checker.source)`. 

Side node: I think `fmt_into` is sort of confusing because `into` generally suggests that it consumes `self` when used as prefix. 

---

_@MichaReiser approved on 2025-01-20 10:14_

Nice, this overall looks good. I didn't review `pep695` in detail because it isn't clear what's copied over and what needs reviewing.

The title suggests to me that UP040 now handles generic functions and classes but reading the summary this doesn't seem to be true? 

What I understand from the summary is that this PR simply introduces a new rule? We should change the title accordingly for a clear changelog entry. 

---

_@AlexWaygood reviewed on 2025-01-20 12:21_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046.py`:9 on 2025-01-20 12:21_

It is valid, but only if `Generic` is the last base class (see https://docs.astral.sh/ruff/rules/generic-not-last-base-class/#why-is-this-bad)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695.rs`:33 on 2025-01-20 12:30_

I find the name `TypeVarKind` (and especially `TypeVarKind::Var`!) a little confusing here. Maybe `TypeParamKind`, and `TypeParamKind::TypeVar`?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:136 on 2025-01-20 12:31_

what's the reason for altering the order from the original class? It seems to work okay to jumble them up at runtime -- couldn't we just keep the order the user has them in?

```pycon
>>> class Foo[**P, T, **Q, *Ts, S]: ...
...
>>>
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:33 on 2025-01-20 12:35_

Hmm, I'm not sure I understand what you're trying to say here. If a function uses PEP-695 type parameters, those parameters are in scope for the whole of the function's body:

```pycon
>>> def f[T]():
...     print(T.__name__)
...     print(type(T))
...     
>>> f()
T
<class 'typing.TypeVar'>
```

And with respect to `isinstance()`, that never worked with old-style `TypeVar`s either!

```pycon
>>> from typing import TypeVar
>>> T = TypeVar("T")
>>> def f(x: T, y: object) -> T:
...     if isinstance(y, T):
...         print('hooray!')
...     return x
...     
>>> f(42, 42)
Traceback (most recent call last):
  File "<python-input-11>", line 1, in <module>
    f(42, 42)
    ~^^^^^^^^
  File "<python-input-10>", line 2, in f
    if isinstance(y, T):
       ~~~~~~~~~~^^^^^^
TypeError: isinstance() arg 2 must be a type, a tuple of types, or a union
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:37 on 2025-01-20 12:35_

```suggestion
/// from typing import TypeVar
/// T = TypeVar("T")
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:59 on 2025-01-20 12:36_

I wonder if these should even be the same rule...? Generic functions and generic classes feel quite distinct.

I don't know how methods play into this, though -- we've obviously left them unimplemented for now, but we might want to try fixing them as a followup extension. We might have to fix methods in the same pass as the class definition...? Not sure...

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:84 on 2025-01-20 12:37_

```suggestion
    // PEP-695 syntax is only available on Python 3.12+
    if checker.settings.target_version < PythonVersion::Py312 {
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:96 on 2025-01-20 12:39_

ooh, maybe we should add another rule for this, if we don't detect it already with something? We could open an issue for it

```pycon
>>> from typing import TypeVar
>>> T = TypeVar("T")
>>> class Foo[S](Generic[T]): ...
... 
Traceback (most recent call last):
  File "<python-input-13>", line 1, in <module>
    class Foo[S](Generic[T]): ...
  File "<python-input-13>", line 1, in <generic parameters of Foo>
    class Foo[S](Generic[T]): ...
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 1279, in _generic_init_subclass
    raise TypeError(
        "Cannot inherit from Generic[...] multiple times.")
TypeError: Cannot inherit from Generic[...] multiple times.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:106 on 2025-01-20 12:40_

I support this -- it's good to cut scope for the first version of the rule. Could you possibly add a note to the rule's documentation explaining this limitation for now, though?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:170 on 2025-01-20 12:42_

Could you add a note to the rule's documentation saying that methods are currently not supported?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:128 on 2025-01-20 12:46_

While this is indeed an error, is it something we should be fixing? It would be just as much an error in the user's original class:

```pycon
>>> class Foo[T, T]: ...
  File "<python-input-14>", line 1
    class Foo[T, T]: ...
                 ^
SyntaxError: duplicate type parameter 'T'
>>> from typing import Generic, TypeVar
>>> T = TypeVar("T")
>>> class Bar(Generic[T, T]): ...
... 
Traceback (most recent call last):
  File "<python-input-16>", line 1, in <module>
    class Bar(Generic[T, T]): ...
              ~~~~~~~^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 432, in inner
    return func(*args, **kwds)
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 1235, in _generic_class_getitem
    raise TypeError(
        f"Parameters to {cls.__name__}[...] must all be unique")
TypeError: Parameters to Generic[...] must all be unique
```

I think instead of filtering out duplicates, we should just bail and not emit a diagnostic if we've found duplicate TypeVars. It will fail at runtime and we can't say for sure what the user wanted to happen.

---

_@AlexWaygood reviewed on 2025-01-20 12:47_

Thanks, this is great!

---

_Comment by @AlexWaygood on 2025-01-20 12:49_

> I was also mildly surprised to see no changes in the ecosystem checks. Do those run with Python 3.12?

I think we run them with Python 3.12 in CI, but when the ecosystem check is running on a project that has `requires-python = ">= 3.9"` in its `pyproject.toml` file, it will assume that that project cannot use Python 3.12 syntax. I think few projects have `requires-python = ">= 3.12"` at this stage, unfortunately, as it's still a fairly new Python version

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:128 on 2025-01-20 13:07_

Ah that makes sense. I copied this from the `non_pep695_type_alias` rule, but it looks like that was to handle cases like this that we don't have to worry about.
https://github.com/astral-sh/ruff/blob/d70d959612d6869c6754717e2333317c06020884/crates/ruff_linter/resources/test/fixtures/pyupgrade/UP040.py#L50-L53

---

_@ntBre reviewed on 2025-01-20 13:07_

---

_@ntBre reviewed on 2025-01-20 13:21_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:33 on 2025-01-20 13:21_

Oh thanks for the clarification! I copied this from UP040 too and thought it applied here. It must be type-alias-specific again.

---

_@ntBre reviewed on 2025-01-20 13:26_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:136 on 2025-01-20 13:26_

I was just trying to match the nice lists from the PEP and `typing` docs :laughing: I agree that keeping the original order makes more sense.

---

_@ntBre reviewed on 2025-01-20 13:37_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:37 on 2025-01-20 13:37_

Thanks! I combined this with the mkdocs fix

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695.rs`:33 on 2025-01-20 13:41_

Sounds good, I also changed the `Tuple` variant to `TypeVarTuple` to match its PEP/`typing` name too.

---

_@ntBre reviewed on 2025-01-20 13:41_

---

_@ntBre reviewed on 2025-01-20 13:54_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046.py`:9 on 2025-01-20 13:54_

I added a test case showing that multiple base classes are not modified currently and also updated the docs to mention this, as Alex suggested below. I'm happy to work on the implementation here if you prefer, but I was planning to handle this in a follow up.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:59 on 2025-01-20 14:04_

Yeah, I was kind of wondering that myself, especially as I was splitting UP040 and this one. The rule function bodies are just different enough that I don't see an easy way of sharing much more code. Should I separate them?

I agree, not sure about methods either. Maybe they fit with functions if we can just subtract the enclosing class's type parameters from the function code (now I'm realizing we might have a problem with nesting in general?). But I could see it going with the class code, the function code, or being a third rule even.

---

_@ntBre reviewed on 2025-01-20 14:04_

---

_@ntBre reviewed on 2025-01-20 14:07_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:17 on 2025-01-20 14:07_

Sounds good, happy to switch. I used these because they're mentioned in the PEP and in the `typing` documentation for the code we're replacing, and UP040 similarly mentions `TypeAlias` and `TypeAliasType`, but more natural language makes sense to me too.

---

_@ntBre reviewed on 2025-01-20 14:16_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695.rs`:1 on 2025-01-20 14:16_

I modified `expr_name_to_type_var` to include the `TypeVar.kind` field, factored out `TypeParam::from` from its call site in `use_pep695_type_alias.rs` and added the `match kind`, and added the `fmt_*` functions. The `Visitor` code is untouched, and so is the `TypeVarRestriction` struct.

Sorry for the confusing mix of changes. I modified it all in-place and then tried splitting it out to separate the rules.

---

_@AlexWaygood reviewed on 2025-01-20 14:18_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:59 on 2025-01-20 14:18_

Ah, nesting is a very good point! I think it's fine to not offer a fix for generic functions or classes nested inside other generic functions or classes (they're rare, and complicated to get right!). You can check whether a class or function is nested inside any class or function scopes by using this method on the semantic model to iterate up through all parent statements and checking if any are a `StmtClassDef` or a `StmtFunctionDef`: https://github.com/astral-sh/ruff/blob/73798327c62257c79effedb2f038a5649911e6b7/crates/ruff_python_semantic/src/model.rs#L1243-L1250

We should definitely add tests for nested functions and classes as well!

For methods -- after thinking about it more, I think we might be okay? Whether or not there's also a fix for the class statement being applied by a different rule simultaneously, I think the same fix needs to be applied for the method definition:

```diff
  T = TypeVar("T")
  S = TypeVar("S")

  class Foo(Generic[T]):
-     def bar(self, x: T, y: S) -> S: ...
+     def bar[S](self, x: T, y: S) -> S: ...
```

or, if the class has also been fixed...

```diff
  S = TypeVar("S")

  class Foo[T]:
-     def bar(self, x: T, y: S) -> S: ...
+     def bar[S](self, x: T, y: S) -> S: ...
```

So I think it _should_ be okay for methods to be their own rule -- which, I think, means that it would be good for free functions and classes to be separate rules too!

---

_@ntBre reviewed on 2025-01-20 15:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP046.py.snap`:8 on 2025-01-20 15:29_

Good call. I thought this looked nice for the examples, but I think your suggestion makes more sense for longer, more realistic class names! And it will be even more important when we handle multiple base classes.

I wanted to make a similar change for functions, but I think it will require tracking the `range` of the parameter in `TypeVar` and possibly even emitting separate diagnostics for each affected parameter. I could cheat a little for classes because I only handle a single base class for now.

I'm picturing a function like this with non-contiguous, unique type parameters as a problematic case:

```python
def h(p: T, another_param, and_another: U): ...
      ^^^^                 ^^^^^^^^^^^^^^
```

I think that's the ideal kind of diagnostic, but I think it requires separate diagnostics and fixes.

---

_@ntBre reviewed on 2025-01-20 15:31_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:84 on 2025-01-20 15:31_

Added this to both checks!

---

_@ntBre reviewed on 2025-01-20 15:36_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:96 on 2025-01-20 15:36_

Issue created! #15620 

---

_@MichaReiser reviewed on 2025-01-20 15:43_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP046.py.snap`:8 on 2025-01-20 15:43_

Hmm yeah that's an interesting case (CC @BurntSushi: While not directly related to diagnostics, it is an interesting problem how we may want multiple diagnostic ranges but a single fix). 

Aren't we tracking the ranges for the fix (by only underlining the type?). although that still doesn't scale to multiple parameters unless we extend the range. 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:17 on 2025-01-20 15:46_

Oops, you're exactly right. These aren't even the PEP/`typing` names. Those are `TypeVar` and `TypeVarTuple` not `TypeParam`. I changed this to 

```rust
/// ## What it does
///
/// Checks for use of standalone type variables and parameter specifications in generic functions
/// and classes.
```

in any case. I also updated both UP046 and the original UP040 docs to use `TypeVar` instead of `TypeParam` in the later variance discussion.

---

_@ntBre reviewed on 2025-01-20 15:46_

---

_@ntBre reviewed on 2025-01-20 17:14_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695.rs`:87 on 2025-01-20 17:14_

Ah great suggestion. I reached for `Display` initially but backed up when I realized I needed the `source` string. The intermediate struct is a good trick to keep in mind, thanks!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046.py`:6 on 2025-01-20 17:20_

We have a test that uses a bound TypeVar here -- could you also add a test that uses a constrained TypeVar?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695.rs`:28 on 2025-01-20 17:21_

nit: with the latest version of the PR, I think this enum no longer needs the `Ord` and `PartialOrd` implementations (and to me there's no "obvious" order they should always go in, so I'd prefer to remove them)

```suggestion
#[derive(Copy, Clone, Debug, Eq, PartialEq)]
```

---

_@ntBre reviewed on 2025-01-20 17:25_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP046.py.snap`:8 on 2025-01-20 17:25_

Yes, in the generic class case, I'm using the assumption of a single base class to underline the type. This is the `range` used later for the diagnostic:
https://github.com/astral-sh/ruff/blob/55be0d8563c2dbcf4126238f6414efc998dc76e1/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs#L109-L116

Once we add a loop to support multiple base classes, like multiple function parameters, I think we'd need to capture each `range` from the parameters as we loop.
https://github.com/astral-sh/ruff/blob/55be0d8563c2dbcf4126238f6414efc998dc76e1/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs#L209-L221

But I could be misunderstanding or missing an easier way to do it.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_alias.rs`:31 on 2025-01-20 17:25_

This issue predates your PR, but the wording here doesn't really make sense to me; I'd change it to

```suggestion
/// `covariant` and `contravariant` keywords used by `TypeVar` variables. As
/// such, rewriting a type alias using a PEP-695 `type` statement may change
/// the variance of the alias's type parameters.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_alias.rs`:36 on 2025-01-20 17:36_

Blegh, the wording is again pretty bad here :/ Your docs for your new rule are much better!

The wording here confuses two issues:
1. Replacing an assignment with a type statement
2. Replacing implicit variance over a globally defined `TypeVar` with an explicit type parameter

For generic old-style type aliases that use type arguments, this whole paragraph actually doesn't apply at all, since you can't use `isinstance()` with them even if they use the old syntax:

```pycon
>>> from typing import TypeVar, TypeAlias
>>> T = TypeVar("T")
>>> X: TypeAlias = list[T]
>>> isinstance([], X)
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    isinstance([], X)
    ~~~~~~~~~~^^^^^^^
TypeError: isinstance() argument 2 cannot be a parameterized generic
```

You can only use `isinstance()` on old-style type aliases if they are _not_ generic:

```pycon
>>> from typing import TypeAlias
>>> X: TypeAlias = int
>>> isinstance(42, X)
True
```

I'd probably go with something like for the docs here

```
/// Unlike type aliases that use simple assignments, definitions created using
/// [PEP 695] `type` statements cannot be used as drop-in replacements at
/// runtime for the value on the right-hand side of the statement. This means
/// that while for some simple old-style type aliases you can use them as the
/// second argument to an `isinstance()` call (for example), doing the same
/// with a [PEP 695] `type` statement will always raise `TypeError` at
/// runtime.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:28 on 2025-01-20 17:38_

```suggestion
/// an inline type parameter may change its variance.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:32 on 2025-01-20 17:40_

```suggestion
/// The rule currently skips generic classes with multiple base classes, and skips
/// generic methods in generic or non-generic classes.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:48 on 2025-01-20 17:41_

Maybe it's worth making the example slightly more realistic by using the type parameter in the class body?

```suggestion
/// class GenericClass(Generic[T]):
///     var: T
/// ```
///
/// Use instead:
///
/// ```python
/// class GenericClass[T]:
///     var: T
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:77 on 2025-01-20 17:43_

Another reason why it might be good for these to be implemented as separate rules: the rules table in the docs [here](https://docs.astral.sh/ruff/rules/) is generated by this method; it will use the contents of the first `format!()` call in this method for this rule's "message" column. Only displaying the first `format!()` call and not the second would give a slightly misleading impression of what the rule does, in this case!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:148 on 2025-01-20 17:46_

nit: to me it makes sense to check the typevars are not empty before filtering out duplicates

```suggestion
    if vars.is_empty() {
        return;
    }

    let type_vars: Vec<&str> = vars
        .iter()
        .unique_by(|tvar| &tvar.name.id)
        .collect();

    // non-unique type variables are runtime errors, so just bail out here
    if type_vars.len() < nvars.len() {
        return;
    }
```

---

_@AlexWaygood reviewed on 2025-01-20 17:48_

---

_@ntBre reviewed on 2025-01-20 17:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695.rs`:28 on 2025-01-20 17:57_

Good call, I mean to make a comment about the ordering, but you've saved me from that too!

---

_Comment by @AlexWaygood on 2025-01-20 18:00_

Oh, could you also possibly add a mention of https://docs.astral.sh/ruff/rules/unused-private-type-var/ to the docs for this rule? The two rules work very well when they're enabled together, as the fixes for this rule might result in lots of type variables becoming unused. This is how you can reference another rule in the docs for a rule: https://github.com/astral-sh/ruff/blob/4cfa3555199f9377b29c490cbb703beb6b1eb3b4/crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs#L116-L119

---

_@ntBre reviewed on 2025-01-20 18:12_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:148 on 2025-01-20 18:12_

This section was bothering me too. I factored it out into a `check_type_vars` function to share it between the class and function rules and applied your suggestion here.

---

_Comment by @AlexWaygood on 2025-01-20 18:43_

Since we don't have a useful ecosystem report on this PR (due to so few projects setting `requires-python` to Python 3.12 or newer), I tried modifying typeshed's pyproject.toml file to say that typeshed only supported Python 3.12+, and I then ran Ruff using this branch (with only UP046 selected) on my local clone of typeshed. Here's a PR to my typeshed fork with the results: https://github.com/AlexWaygood/typeshed/pull/34.

Summary of the findings:
1. No syntax errors or comments dropped as far as I can see -- it mostly looks fantastic!

2. However, mypy crashes. Given that pyright is basically fine with all the changes here, this is almost certainly a bug in mypy, and probably a typeshed-specific one (typeshed provides the stubs for some very core parts of the standard library, which mypy often makes assumptions about; it's quite easy to provoke a mypy crash by changing random parts of typeshed). Given that it's quite unlikely that this will affect non-typeshed users, I think this probably doesn't matter for us.

3. Pyright _nearly_ passes, but there's a few errors about missing type arguments for `slice`. And this highlights an important point that we both missed! `slice` is defined [here](https://github.com/python/typeshed/blob/57d7c4334b64856fda6f6e8f992b101ddafe2f57/stdlib/builtins.pyi#L943) in typeshed, and we can see it's generic over three type parameters. The type parameters are defined [here](https://github.com/python/typeshed/blob/57d7c4334b64856fda6f6e8f992b101ddafe2f57/stdlib/builtins.pyi#L98-L100) -- and they all have `default` keyword arguments used in their definitions! This is [PEP 696](https://peps.python.org/pep-0696/), which was introduced in Python 3.13. An old-style TypeVar that uses the `default` keyword cannot be rewritten with type parameters on Python 3.12, but if we're on Python 3.13+, we can rewrite it like this:

   ```diff
     from typing_extensions import Any, TypeVar, Generic

     T = TypeVar("T", default=Any, bound=str)

   - class slice(Generic[T]):
   + class slice[T: str = Any]:
         start: T
   ```

4. It's a bit annoying that the rule will rewrite something like this:
   
   ```py
   _T = TypeVar("_T")

   def f(x: _T) -> _T: ...
   ```

   to

   ```py
   def f[_T](x: _T) -> _T: ...
   ```

   when what I'd really _like_ is

   ```py
   def f[T](x: T) -> T: ...
   ```

   The leading underscore is just unnecessary visual noise when it becomes a type parameter,
since type parameters can't anyway be accessed from outside the function scope; they're private by default. I don't think we should try to rename any type variables as part of this PR, but we might consider thinking about this as part of a followup.

---

_@ntBre reviewed on 2025-01-20 18:43_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046.py`:6 on 2025-01-20 18:43_

Good catch, I added one for classes and one for functions!

---

_Comment by @ntBre on 2025-01-20 18:59_

Wow great catch on the `default` argument. Should we just bail out for now if we see a `default` argument? Eventually I guess we'll need a bit more bookkeeping around the `checker.settings.target_version`.

Otherwise I think the remaining TODOs are 
- [x] add tests for nested functions and classes (and bail out of nested scopes)
- [x] split into two rules

Hopefully I've addressed everything else, but please let me know if I missed something!

---

_Comment by @AlexWaygood on 2025-01-20 19:01_

> Should we just bail out for now if we see a `default` argument?

Yup, fine to bail out on seeing a `default` argument for this PR! (Though we should make a note about that in the docs for the rule, or we'll get bug reports about it if we land this and then never get to the followup implementing support for `TypeVar`s that have the `default` argument

---

_Renamed from "[`pyupgrade`] Expand UP040 to handle generic functions and classes (`UP040`, `UP046`)" to "[`pyupgrade`] Add rules to use PEP 695 generics in classes and functions (`UP046`, `UP047`)" by @ntBre on 2025-01-20 21:29_

---

_Comment by @ntBre on 2025-01-20 21:46_

Okay, I think I covered all of the suggestions! For follow ups I have

- [ ] Rename private type variables in generics (don't make inline generics like `[_T]`)
- [ ] Allow multiple base classes for generic classes
- [ ] Handle generic methods
- [ ] Handle `default` kwargs for Python >= 3.13

Plus the separate, related rule for mixed old- and new-style generics in #15620. Should I open issues for each of these, or just track them here?

---

_@MichaReiser reviewed on 2025-01-21 07:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:59 on 2025-01-21 07:50_

Sorry for being late to the party but can you elaborate a bit on the reason why we chose to split the rule? The reason for the split aren't clear to me as a user. What's so different between classes and functions that I'd only want this rule enabled for one or the other? I don't think it should matter how much code the implementations can share -- that would be an us problem and we shouldn't forward it to the users by "overwhelming" them with more rules".

---

_Comment by @MichaReiser on 2025-01-21 07:53_

I'd suggest creating a new issue. They all seem like extensions and can be implemented after this PR.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:291 on 2025-01-21 13:38_

we can avoid allocating a `Vec` here (which might be expensive) and also simplify a little:

```suggestion
    // If any type varaibles were not unique, just bail out here
    // this is a runtime error and we can't predict what the user wanted
    (vars.iter().unique_by(|tvar| &tvar.name.id).count() == vars.len()).then_some(vars)
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_generic_class.rs`:25 on 2025-01-21 13:38_

```suggestion
/// `contravariant` keywords used by `TypeVar` variables. As such, replacing a `TypeVar` variable
/// with an inline type parameter may change its variance.
```

---

_@ntBre reviewed on 2025-01-21 13:39_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:59 on 2025-01-21 13:39_

Another reason Alex mentioned [down below](https://github.com/astral-sh/ruff/pull/15565#discussion_r1922718333) was that the rules table in the docs uses the first `format!` call from `Violation::message`, so splitting them will give two more specialized descriptions in that table. Otherwise, we discussed that it's generally nice to lean toward more fine-grained rule selection, but I see what you mean about overwhelming users with rules. I also think you're right that users will probably want both of these together, but @AlexWaygood may have a different intuition here. I'm happy to go either way.

I think this is a side note related to rule categorization, but my ideal picture is something like a PEP 695 parent category that would activate all three of these (UP040, UP046, and UP047). That's why I put them all in UP040 initially, but the naming of UP040 ([non-pep695-type-alias](https://docs.astral.sh/ruff/rules/non-pep695-type-alias/#non-pep695-type-alias-up040)) was a bit too specific to include the others.

If we do leave them split, you've reminded me that I should add a `See also` section to both of them pointing to the other at least.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_generic_class.rs`:28 on 2025-01-21 13:39_

I think the note about generic methods is irrelevant here now that it's been split up into two rules

```suggestion
/// The rule currently skips generic classes with multiple base classes. It also skips
/// generic classes nested inside of other
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_generic_class.rs`:53 on 2025-01-21 13:40_

```suggestion
/// This rule replaces standalone type variables in classes but doesn't remove
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_generic_function.rs`:25 on 2025-01-21 13:41_

```suggestion
/// `contravariant` keywords used by `TypeVar` variables. As such, replacing a `TypeVar` variable
/// with an inline type parameter may change its variance.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_generic_function.rs`:47 on 2025-01-21 13:42_

```suggestion
/// def generic_function(var: T) -> T:
///     return var
/// ```
///
/// Use instead:
///
/// ```python
/// def generic_function[T](var: T) -> T:
///     return var
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_generic_function.rs`:52 on 2025-01-21 13:42_

```suggestion
/// This rule replaces standalone type variables in function signatures but doesn't remove
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_generic_function.rs`:55 on 2025-01-21 13:43_

```suggestion
/// [`unused-private-type-var`](unused-private-type-var.md) for a rule to clean up unused
/// private type variables.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_generic_class.rs`:56 on 2025-01-21 13:43_

```suggestion
/// [`unused-private-type-var`](unused-private-type-var.md) for a rule to clean up unused
/// private type variables.
```

---

_@AlexWaygood reviewed on 2025-01-21 13:44_

This looks very close!

---

_Comment by @AlexWaygood on 2025-01-21 13:45_

> I'd suggest creating a new issue. They all seem like extensions and can be implemented after this PR.

Agree -- I think a single new issue that tracks all these followups as sub-items would be perfect

---

_@ntBre reviewed on 2025-01-21 13:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:291 on 2025-01-21 13:50_

Oh nice! I always forget about the methods on `bool`.

---

_Comment by @ntBre on 2025-01-21 14:04_

Thanks for all of the documentation fixes and the `then_some` improvement! It looks like there's a tiny typo that I'm fixing locally. Otherwise, do we want to [revisit](https://github.com/astral-sh/ruff/pull/15565#discussion_r1923216394) splitting the rule? If not, I just want to extend the `See also` sections to point to each other. I opened the other issue for follow ups too.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:59 on 2025-01-21 14:06_

I saw the format issue. While not ideal, I don't think it's reason enough to split a rule because I consider having to configure two rules more inconvenient than a slightly confusing message in the rules table (which we can fix when we refactor Ruff's rule infrastructure to Red Knot's)

---

_@MichaReiser reviewed on 2025-01-21 14:06_

---

_@AlexWaygood reviewed on 2025-01-21 14:11_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:59 on 2025-01-21 14:11_

Personally I think that these are doing distinct enough things that they deserve to be separate rules -- the documentation examples for them are completely different, for example. I do see Micha's point that there's maybe not really a reason for why somebody might want to enable one but leave the other disabled -- but in general, I feel like we see a lot of requests for fine-grained configuration on the issue tracker, but not much demand for coarser configuration. I think there are many confusing things about Ruff's configuration setup, but I don't think the sheer number of rules is one of them... but I know that this is somewhere where Micha and I disagree :-)

---

_Comment by @AlexWaygood on 2025-01-21 14:18_

I updated the [PR against my typeshed fork](https://github.com/AlexWaygood/typeshed/pull/34) to run the autofixes from the latest version of this PR. The pyright CI there is flagging a couple of other issues:
1. `/home/runner/work/typeshed/typeshed/stdlib/builtins.pyi:1547:32 - error: Type parameter "SupportsRichComparisonT" is not included in the type parameter list for "max" (reportGeneralTypeIssues)`.
   
   This is because `SupportsRichComparisonT` is defined in another module, so we can't see that it's a type variable. This actually means that the autofix breaks the function definition, because the function now mixes old-style type variable declarations with new-style type parameters in its annotations, and pyright correctly rejects this. I don't think there's a good solution here other than to make sure that the fix is marked unsafe, and to document that this is a known limitation of the rule.

2. In a similar vein, it looks like we don't currently recognise `typing.AnyStr` as being a type variable. This is the same issue as issue (1), but it's a very commonly used type variable (and a public member of the standard-library `typing` module), so I think it's worth us adding some special-casing for it so that it's recognised as a type variable.


---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_generic_class.rs`:156 on 2025-01-21 14:22_

the fixes to these rules should be marked as unsafe rather than safe:
- The issue pointed out in the `Known issues` section regarding the switch from explicit variance to inferred variance means that they could change the semantics of the code's annotations
- the issue I pointed out just now regarding type variables from other modules means that the fixes could in some cases outright break the code's annotations

```suggestion
        .with_fix(Fix::unsafe_edit(Edit::replacement(
            type_params.to_string(),
            name.end(),
            arguments.end(),
        ))),
```

We also need to add a `## Fix safety` section to the rules' docs explaining why the fixes are marked as unsafe

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_generic_function.rs`:158 on 2025-01-21 14:23_

```suggestion
        .with_fix(Fix::unsafe_edit(Edit::insertion(
            type_params.to_string(),
            name.end(),
        ))),
```

---

_@AlexWaygood reviewed on 2025-01-21 14:23_

---

_@AlexWaygood reviewed on 2025-01-21 14:26_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_generic_class.rs`:156 on 2025-01-21 14:26_

(if you grep for `## Fix safety`, you can find lots of other rules that have similar sections in their docs, that you can use as examples!)

---

_@MichaReiser reviewed on 2025-01-21 14:55_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_parameter.rs`:59 on 2025-01-21 14:55_

>  but I don't think the sheer number of rules is one of them

I don't think this is necessarily true, given that users ask for presets, better categorization etc. Users struggle with configuring Ruff and the sheer amount of rules is one big factor causing it. Just speaking for myself, finding if a rule already exists is no easy task (you can try pasting it in the playground but good luck if the rule requires configuration). You have to scroll through a sheer endless list of rules. And I think it's a problem we don't understand well yet because users simply tend to not enable rules because they're overwhelmed (or enable all of them) -- giving them a worse experience overall. That's why I keep pushing back on defaulting to more granular rules. There's a non zero cost. 


Looking at the two rules and reading the documentation I think either works. I've a slight preference to just have one, but considering that we split them out from UP40, maybe it's makes sense to have them separate. I'd probably go with separate rules just to avoid extra work.


---

_Comment by @ntBre on 2025-01-21 20:19_

Thanks again for the thorough review and for your `typeshed` tests! I think I have now
* handled imported type variables (by giving only a diagnostic in classes and by giving an unsafe fix (that excludes them) in functions)
* updated the `Known issues` and `Fix safety` sections of both rules
* added a special case for `AnyStr`

The `AnyStr` case felt a little awkward, so hopefully it's at least close to what you had in mind.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046.py`:52 on 2025-01-21 21:30_

micro-nit: it's nice to keep fixtures looking like "realistic" Python code where possible (this looks a bit strange to me, as there's generally no reason to use a `TypeVar` in a function's annotations like this unless either two parameters are annotated with the `TypeVar` or the return annotation uses the `TypeVar`):

```suggestion
    def generic_method(var: T) -> T:
        return var
```

this applies to a few other functions and classes in your fixtures, but I won't comment on all of them ;)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046.py`:28 on 2025-01-21 21:33_

should `SupportsRichComparisonT` be imported from another module at the top of this file?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:111 on 2025-01-21 21:57_

hrm... I'm not a huge fan of the way that the special handling for `AnyStr` is kind-of implicit here. It also seems a shame that we're now cloning all `TypeVarRestriction::Constraint` `Expr`s just to handle the `AnyStr` special case.

I think we can solve this by:
1. Adding a dedicated variant for `TypeVarRestriction`, and
2. Only storing an `&'a str` for the `name` field on `TypeVar`, rather than a whole `ast::name::Name`.

Something like this? I think this patch also has the advantage that we'd recognise a fully qualified use of `AnyStr` as an attribute expression, e.g. `import typing; x: typing.AnyStr`:

<details>
<summary>Suggested patch</summary>

```diff
diff --git a/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs b/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs
index 3de4228bc..de9a6407a 100644
--- a/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs
+++ b/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs
@@ -28,7 +28,10 @@ enum TypeVarRestriction<'a> {
     /// A type variable with a bound, e.g., `TypeVar("T", bound=int)`.
     Bound(&'a Expr),
     /// A type variable with constraints, e.g., `TypeVar("T", int, str)`.
-    Constraint(Vec<Expr>),
+    Constraint(Vec<&'a Expr>),
+    /// `AnyStr` is a special case: the only public `TypeVar` defined in the standard library,
+    /// and thus the only one that we recognise when imported from another module.
+    AnyStr,
 }
 
 #[derive(Copy, Clone, Debug, Eq, PartialEq)]
@@ -40,7 +43,7 @@ enum TypeParamKind {
 
 #[derive(Debug)]
 struct TypeVar<'a> {
-    name: &'a ExprName,
+    name: &'a str,
     restriction: Option<TypeVarRestriction<'a>>,
     kind: TypeParamKind,
     default: Option<&'a Expr>,
@@ -92,23 +95,19 @@ impl Display for DisplayTypeVar<'_> {
             TypeParamKind::TypeVarTuple => f.write_str("*")?,
             TypeParamKind::ParamSpec => f.write_str("**")?,
         }
-        f.write_str(&self.type_var.name.id)?;
+        f.write_str(self.type_var.name)?;
         if let Some(restriction) = &self.type_var.restriction {
             f.write_str(": ")?;
             match restriction {
                 TypeVarRestriction::Bound(bound) => {
                     f.write_str(&self.source[bound.range()])?;
                 }
+                TypeVarRestriction::AnyStr => f.write_str("(bytes, str)")?,
                 TypeVarRestriction::Constraint(vec) => {
                     let len = vec.len();
                     f.write_str("(")?;
                     for (i, v) in vec.iter().enumerate() {
-                        // typing.AnyStr special case doesn't have a real range
-                        if let Expr::Name(name) = v {
-                            f.write_str(name.id.as_ref())?;
-                        } else {
-                            f.write_str(&self.source[v.range()])?;
-                        }
+                        f.write_str(&self.source[v.range()])?;
                         if i < len - 1 {
                             f.write_str(", ")?;
                         }
@@ -135,7 +134,7 @@ impl<'a> From<&'a TypeVar<'a>> for TypeParam {
             TypeParamKind::TypeVar => {
                 TypeParam::TypeVar(TypeParamTypeVar {
                     range: TextRange::default(),
-                    name: Identifier::new(name.id.clone(), TextRange::default()),
+                    name: Identifier::new(*name, TextRange::default()),
                     bound: match restriction {
                         Some(TypeVarRestriction::Bound(bound)) => Some(Box::new((*bound).clone())),
                         Some(TypeVarRestriction::Constraint(constraints)) => {
@@ -146,6 +145,25 @@ impl<'a> From<&'a TypeVar<'a>> for TypeParam {
                                 parenthesized: true,
                             })))
                         }
+                        Some(TypeVarRestriction::AnyStr) => {
+                            Some(Box::new(Expr::Tuple(ast::ExprTuple {
+                                range: TextRange::default(),
+                                elts: vec![
+                                    Expr::Name(ExprName {
+                                        range: TextRange::default(),
+                                        id: Name::from("str"),
+                                        ctx: ast::ExprContext::Load,
+                                    }),
+                                    Expr::Name(ExprName {
+                                        range: TextRange::default(),
+                                        id: Name::from("bytes"),
+                                        ctx: ast::ExprContext::Load,
+                                    }),
+                                ],
+                                ctx: ast::ExprContext::Load,
+                                parenthesized: true,
+                            })))
+                        }
                         None => None,
                     },
                     // We don't handle defaults here yet. Should perhaps be a different rule since
@@ -155,12 +173,12 @@ impl<'a> From<&'a TypeVar<'a>> for TypeParam {
             }
             TypeParamKind::TypeVarTuple => TypeParam::TypeVarTuple(TypeParamTypeVarTuple {
                 range: TextRange::default(),
-                name: Identifier::new(name.id.clone(), TextRange::default()),
+                name: Identifier::new(*name, TextRange::default()),
                 default: None,
             }),
             TypeParamKind::ParamSpec => TypeParam::ParamSpec(TypeParamParamSpec {
                 range: TextRange::default(),
-                name: Identifier::new(name.id.clone(), TextRange::default()),
+                name: Identifier::new(*name, TextRange::default()),
                 default: None,
             }),
         }
@@ -178,36 +196,27 @@ struct TypeVarReferenceVisitor<'a> {
 /// Recursively collects the names of type variable references present in an expression.
 impl<'a> Visitor<'a> for TypeVarReferenceVisitor<'a> {
     fn visit_expr(&mut self, expr: &'a Expr) {
+        // special case for typing.AnyStr, which is a commonly-imported type variable in the
+        // standard library with the definition:
+        //
+        // ```python
+        // AnyStr = TypeVar('AnyStr', bytes, str)
+        // ```
+        //
+        // As of 01/2025, this line hasn't been modified in 8 years, so hopefully there won't be
+        // much to keep updated here. See
+        // https://github.com/python/cpython/blob/383af395af828f40d9543ee0a8fdc5cc011d43db/Lib/typing.py#L2806
+        if self.semantic.match_typing_expr(expr, "AnyStr") {
+            self.vars.push(TypeVar {
+                name: "AnyStr",
+                restriction: Some(TypeVarRestriction::AnyStr),
+                kind: TypeParamKind::TypeVar,
+                default: None,
+            });
+            return;
+        }
+
         match expr {
-            // special case for typing.AnyStr, which is a commonly-imported type variable in the
-            // standard library with the definition:
-            //
-            // ```python
-            // AnyStr = TypeVar('AnyStr', bytes, str)
-            // ```
-            //
-            // As of 01/2025, this line hasn't been modified in 8 years, so hopefully there won't be
-            // much to keep updated here. See
-            // https://github.com/python/cpython/blob/383af395af828f40d9543ee0a8fdc5cc011d43db/Lib/typing.py#L2806
-            e @ Expr::Name(name) if self.semantic.match_typing_expr(e, "AnyStr") => {
-                self.vars.push(TypeVar {
-                    name,
-                    restriction: Some(TypeVarRestriction::Constraint(vec![
-                        Expr::Name(ExprName {
-                            range: TextRange::default(),
-                            id: Name::from("bytes"),
-                            ctx: ruff_python_ast::ExprContext::Load,
-                        }),
-                        Expr::Name(ExprName {
-                            range: TextRange::default(),
-                            id: Name::from("str"),
-                            ctx: ruff_python_ast::ExprContext::Load,
-                        }),
-                    ])),
-                    kind: TypeParamKind::TypeVar,
-                    default: None,
-                });
-            }
             Expr::Name(name) if name.ctx.is_load() => {
                 if let Some(var) = expr_name_to_type_var(self.semantic, name) {
                     self.vars.push(var);
@@ -243,7 +252,7 @@ fn expr_name_to_type_var<'a>(
         }) => {
             if semantic.match_typing_expr(subscript_value, "TypeVar") {
                 return Some(TypeVar {
-                    name,
+                    name: &name.id,
                     restriction: None,
                     kind: TypeParamKind::TypeVar,
                     default: None,
@@ -288,14 +297,14 @@ fn expr_name_to_type_var<'a>(
                     Some(TypeVarRestriction::Bound(&bound.value))
                 } else if arguments.args.len() > 1 {
                     Some(TypeVarRestriction::Constraint(
-                        arguments.args.iter().skip(1).cloned().collect(),
+                        arguments.args.iter().skip(1).collect(),
                     ))
                 } else {
                     None
                 };
 
                 return Some(TypeVar {
-                    name,
+                    name: &name.id,
                     restriction,
                     kind,
                     default,
@@ -327,7 +336,7 @@ fn check_type_vars(vars: Vec<TypeVar<'_>>) -> Option<Vec<TypeVar<'_>>> {
     // found on the type parameters
     (vars
         .iter()
-        .unique_by(|tvar| &tvar.name.id)
+        .unique_by(|tvar| tvar.name)
         .filter(|tvar| tvar.default.is_none())
         .count()
         == vars.len())
diff --git a/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_alias.rs b/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_alias.rs
index c2340c609..1fb63f8be 100644
--- a/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_alias.rs
+++ b/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/use_pep695_type_alias.rs
@@ -133,7 +133,7 @@ pub(crate) fn non_pep695_type_alias_type(checker: &mut Checker, stmt: &StmtAssig
         .map(|expr| {
             expr.as_name_expr().map(|name| {
                 expr_name_to_type_var(checker.semantic(), name).unwrap_or(TypeVar {
-                    name,
+                    name: &name.id,
                     restriction: None,
                     kind: TypeParamKind::TypeVar,
                     default: None,
@@ -199,7 +199,7 @@ pub(crate) fn non_pep695_type_alias(checker: &mut Checker, stmt: &StmtAnnAssign)
     // Type variables must be unique; filter while preserving order.
     let vars = vars
         .into_iter()
-        .unique_by(|TypeVar { name, .. }| name.id.as_str())
+        .unique_by(|tvar| tvar.name)
         .collect::<Vec<_>>();
 
     checker.diagnostics.push(create_diagnostic(
```

</details>

---

_@AlexWaygood reviewed on 2025-01-21 21:58_

---

_@AlexWaygood reviewed on 2025-01-21 22:01_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:173 on 2025-01-21 22:01_

Should we also add a test for UP040 with a type alias that uses a `TypeVar` with a default (since that rule now shares the logic from this module)?

We might want to see if we can convert the fix there to also use simple string interpolation rather than the `ExprGenerator`... I feel like it makes sense for all three rules to use basically the same infrastructure. But not for this PR! Just another possible followup for you ;)

---

_@AlexWaygood reviewed on 2025-01-21 22:08_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:206 on 2025-01-21 22:08_

I think we have to make sure here that `str` and `bytes` haven't been overridden in an enclosing scope before we add a type parameter that uses `(str, bytes)` as constraints. E.g. <https://mypy-play.net/?mypy=latest&python=3.12&gist=100810bb27581625b327bce54ec1b63b>. If `str` and/or `bytes` have been overridden with symbols in an enclosing scope, we can't offer a fix to replace `AnyStr` with an inline type parameter. I think you can check for this using the `SemanticModel::is_available()` method. https://github.com/astral-sh/ruff/blob/ef85c682bdd76ed63457bdfbec72e09424ac20ab/crates/ruff_python_semantic/src/model.rs#L327-L331

---

_@AlexWaygood reviewed on 2025-01-21 22:10_

The docs look fantastic now! Thanks again for your patience!

---

_@AlexWaygood reviewed on 2025-01-22 10:17_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:206 on 2025-01-22 10:17_

To be clear: I'd _only_ do this for `AnyStr`, at least for the first version of this rule. I think theoretically it could apply to the bounds or constraints for any `TypeVar`, but:
1. It's much more likely to be an issue for a TypeVar coming from another module like `AnyStr`
2. I think it'll just be much more complicated to account for name shadowing with arbitrary non-`AnyStr` TypeVars, and it's probably not essential since the fix is already marked as unsafe.

---

_Comment by @AlexWaygood on 2025-01-22 11:10_

I did some more testing using the PR to my typeshed fork. I have good news and bad news!

The good news is that the pyright report on the typeshed PR is now basically clean -- all remaining errors are due to TypeVars coming from other modules, but we've flagged that in the rule's documentation. Also, these just aren't that common, so there aren't very many errors relating to even that.

The bad news is that I managed to minimize the mypy crash, and it doesn't actually look like something that would only affect typeshed: https://github.com/python/mypy/issues/18507. I think it could potentially affect any stub authors. Pytype is also crashing on the PR to my typeshed fork, so we should perhaps add a note to this rule's documentation saying that not all type checkers fully support PEP 695 yet, so users may need to be careful before applying the rule's autofixes to their codebase.

---

_@ntBre reviewed on 2025-01-22 13:46_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046.py`:52 on 2025-01-22 13:46_

Ah sorry for making you repeat yourself. I know you mentioned it for the docs, but I thought I could get away with it for the tests! I'll keep this in mind now.

---

_@ntBre reviewed on 2025-01-22 13:54_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046.py`:28 on 2025-01-22 13:54_

I didn't know where it's actually from (maybe `_typeshed` from a github search?), but I imported it from `somewhere` at the top to make it more clear that we're testing an unknown type variable,  not just a missing import. Thanks!

---

_@AlexWaygood reviewed on 2025-01-22 13:56_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046.py`:28 on 2025-01-22 13:56_

yup, it's defined in `_typeshed` here: https://github.com/python/typeshed/blob/ce521e8c76eec84828b30353b163fcff8e054bbd/stdlib/_typeshed/__init__.pyi#L104 (it doesn't actually exist at runtime!). But importing it from `somewhere` is fine for these fixtures; the only important thing is that it needs to come from another module 👍

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:111 on 2025-01-22 13:57_

Agreed, I didn't like it either. Thanks for the patch!

---

_@ntBre reviewed on 2025-01-22 13:57_

---

_@AlexWaygood reviewed on 2025-01-22 14:01_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:206 on 2025-01-22 14:01_

Oh, actually, `SemanticModel::has_builtin_binding` might be better for this use case that `SemanticModel::is_available`:

https://github.com/astral-sh/ruff/blob/13e7afca42e1ab4e698412df903e3dd172909caa/crates/ruff_python_semantic/src/model.rs#L268-L277

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:173 on 2025-01-22 14:22_

Added some tests and three lines to bail out if we see `default`, and I added the string suggestion to the follow up issue.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:1 on 2025-01-22 14:24_

And UP047!

---

_@AlexWaygood reviewed on 2025-01-22 14:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:206 on 2025-01-22 14:41_

Great catch! I also added a test for this. Is there a way to reset the builtin status of `str`, for example? I tried this initially to check both `str` and `bytes` and that everything was reset, but I think any assignment to a builtin might just mean it's not built in anymore? And we can't nest within a class or function to get a new scope since we skip nesting too! I'm happy enough with the current test, which is just the first case here, but I wanted to check.

```python
str = "string"
class BadStr(Generic[AnyStr]):
    var: AnyStr

str = __builtins__.str

bytes = b"bytes"
class BadBytes(Generic[AnyStr]):
    var: AnyStr

bytes = __builtins__.bytes

class OkayAgain(Generic[AnyStr]):
    var: AnyStr
```

---

_@ntBre reviewed on 2025-01-22 14:45_

---

_@AlexWaygood reviewed on 2025-01-22 14:49_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:206 on 2025-01-22 14:49_

> Is there a way to reset the builtin status of `str`, for example?

Ooh good point. Usually I'd usually put everything inside a function but here that obviously doesn't work, for the reasons you pointed out! Maybe you could put the test in a different fixture file entirely (with a docstring at the top of the fixture file saying why it's separated from most of the rest of the fixtures for these rules)?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:206 on 2025-01-22 14:58_

Ah okay, I just put it at the bottom, but a separate file probably makes more sense!

---

_@ntBre reviewed on 2025-01-22 14:58_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046_0.py`:29 on 2025-01-22 15:06_

```suggestion
# or constraints on the TypeVar imported from another module
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046_0.py`:41 on 2025-01-22 15:08_

for a `TypeVarTuple`, this would either be

```suggestion
class MultipleGenerics(Generic[S, T, *Ts, P]):
```

or, if the code was originally written to support Python <3.11

```suggestion
class MultipleGenerics(Generic[S, T, Unpack[Ts], P]):
```

with `Unpack` imported from `typing` or `typing_extensions`. Proper handling of `Unpack` is yet another thing you could possibly handle in a followup PR 😅

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:3 on 2025-01-22 15:12_

Hmm... I suppose these new rules don't really fit with our [naming convention](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#rule-naming-convention). On the other hand, they do match the name of UP040, which is a very similar pre-existing rule. Not sure whether we should use our "general convention" or the "local convention" set by UP040... @MichaReiser, do you have any preference?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:172 on 2025-01-22 15:17_

(I'm not sure it should be a different _rule_ but let's discuss that on https://github.com/astral-sh/ruff/issues/15642 ;)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:254 on 2025-01-22 15:25_

I know this is just copied over from where it was before -- but we can simplify this quite a bit with some `?`s:

```suggestion
    let StmtAssign { value, .. } = semantic
        .lookup_symbol(name.id.as_str())
        .and_then(|binding_id| semantic.binding(binding_id).source)
        .map(|node_id| semantic.statement(node_id))?
        .as_assign_stmt()?;
```

---

_@AlexWaygood approved on 2025-01-22 15:26_

Brilliant work. This looks ready to land now!

---

_@ntBre reviewed on 2025-01-22 15:30_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:3 on 2025-01-22 15:30_

I think the rule names are actually okay, but the modules don't match the rule names :sweat_smile: I think that should be an easy and non-user-facing change if we wanted to rename all three?

https://github.com/astral-sh/ruff/blob/9fadf46933bb5cc7f6eafbcd22418dfdd8021e3a/crates/ruff_linter/src/codes.rs#L537

https://github.com/astral-sh/ruff/blob/9fadf46933bb5cc7f6eafbcd22418dfdd8021e3a/crates/ruff_linter/src/codes.rs#L543-L544

---

_@AlexWaygood reviewed on 2025-01-22 15:31_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:3 on 2025-01-22 15:31_

Ohh perfect. Yes, let's just rename the files to match the rules that are in them, then 😄

---

_@ntBre reviewed on 2025-01-22 15:32_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:172 on 2025-01-22 15:32_

I'll blame Zanie for that one since it was already there! But sounds good to discuss in any case :)

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046_0.py`:41 on 2025-01-22 15:39_

Ah, I thought I caught all of these this time. Thanks!

---

_@ntBre reviewed on 2025-01-22 15:39_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:2 on 2025-01-22 15:58_

```suggestion
//! Shared code for [`non_pep695_type_alias`] (UP040),
//! [`non_pep695_generic_class`] (UP046), and [`non_pep695_generic_function`]
```

---

_@AlexWaygood approved on 2025-01-22 15:59_

---

_@ntBre reviewed on 2025-01-22 16:04_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:2 on 2025-01-22 16:04_

Ahh, the one place my LSP couldn't rename for me. I did a quick grep this time, and there's also a pyflakes test called `use_pep695_type_aliass` (double `s` included). Should I rename that as well?

---

_@AlexWaygood reviewed on 2025-01-22 16:12_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:2 on 2025-01-22 16:12_

Hmm, it looks like that pyflakes test was only _renamed_ in https://github.com/astral-sh/ruff/pull/6289/files... and I'm not sure why, because it doesn't seem relevant to that PR (the type aliases in that test don't use PEP 695, and there are no changes to any pyflakes-related code in that PR other than the test being renamed). It looks like it _might_ have been renamed by mistake?

@zanieb, I know it was a while ago, but I don't suppose you have any context on why that test was renamed in that PR, do you? :-)

I don't think the naming of that separate pyflakes test needs to hold up this PR being merged, though 😄

---

_@ntBre reviewed on 2025-01-22 16:16_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:2 on 2025-01-22 16:16_

Sounds good! I'll push this fix and then move towards merging.

---

_Merged by @ntBre on 2025-01-22 16:35_

---

_Closed by @ntBre on 2025-01-22 16:35_

---

_Branch deleted on 2025-01-22 16:35_

---
