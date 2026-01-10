```yaml
number: 13719
title: "[red-knot] Port type inference tests to new test framework"
type: pull_request
state: merged
author: Lexxxzy
labels:
  - ty
assignees: []
merged: true
base: main
head: type-infer-tests
created_at: 2024-10-11T18:32:18Z
updated_at: 2024-10-15T19:41:40Z
url: https://github.com/astral-sh/ruff/pull/13719
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] Port type inference tests to new test framework

---

_Pull request opened by @Lexxxzy on 2024-10-11 18:32_

## Summary

Porting infer tests to new markdown tests framework.

Link to the corresponding issue: #13696 


---

_Review requested from @carljm by @Lexxxzy on 2024-10-11 18:32_

---

_Review requested from @MichaReiser by @Lexxxzy on 2024-10-11 18:32_

---

_Review requested from @AlexWaygood by @Lexxxzy on 2024-10-11 18:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/imports.md`:16 on 2024-10-11 18:35_

Oh, the outer markdown fenced code block is just how the mdtest README handles showing an example markdown document within a markdown README, it's not needed in the tests and should be removed here and in all other cases:

````suggestion
```py path=a.py
from b import C as D; E = D
reveal_type(E) # revealed: Literal[C]
```

```py path=b.py
class C: pass
```
````

It's funny that it still works either way, because of our naive regex parsing :)

I'll look at updating the README to address this potential confusion.

---

_Comment by @Lexxxzy on 2024-10-11 18:36_

I've changed parser's CODE_RE regex, cause without that fix it was not correctly parsing code snippets with empty files, like:

````markdown
```py path=package/__init__.py
```
````

Of course feel free to comment on this fix...

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/imports.md`:8 on 2024-10-11 18:36_

I would say it's probably better style in general if we don't specify the path on the "main" test file, if it isn't imported by another file and its location/name don't actually matter to the test. (This wasn't possible in the previous test format.) I don't care too much about this, though, not a blocking issue.

---

_@carljm requested changes on 2024-10-11 18:41_

This is awesome, thank you for taking this on!

I'm going to wait for removal of the extraneous quadruple-backtick markdown fences before reviewing the tests in detail, because removing those will make them a lot easier to read :)

---

_Comment by @carljm on 2024-10-11 18:42_

> I've changed parser's CODE_RE regex

This fix looks good, thank you!

---

_Comment by @carljm on 2024-10-11 18:44_

Also it looks like the mdformat pre-commit check is failing, you can check the ruff CONTRIBUTING doc for info on how to run pre-commit locally so you can catch those issues without having to push and wait for CI.

---

_Comment by @github-actions[bot] on 2024-10-11 18:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @Lexxxzy on 2024-10-11 18:50_

> wait for removal of the extraneous quadruple-backtick

Oh, ok, understood, I will redo that, thanks for clarifing

> Also it looks like the mdformat pre-commit check is failing, you can check the ruff CONTRIBUTING doc for info on how to run pre-commit locally so you can catch those issues without having to push and wait for CI.

Got it, totally forgot about it

---

_Label `red-knot` added by @carljm on 2024-10-11 19:01_

---

_@Lexxxzy reviewed on 2024-10-12 10:11_

---

_Review comment by @Lexxxzy on `crates/red_knot_python_semantic/resources/mdtest/imports.md`:8 on 2024-10-12 10:11_

Fixed it

---

_@Lexxxzy reviewed on 2024-10-12 10:13_

---

_Review comment by @Lexxxzy on `crates/red_knot_python_semantic/resources/mdtest/imports.md`:16 on 2024-10-12 10:13_

Fixed, my bad. Once again - thanks for clarifying

---

_Comment by @Lexxxzy on 2024-10-12 10:27_

Rebased on the current `main` branch. Changed format of tests after new clarifications from @carljm. Also added tests for booleans infer.

I have problems with porting few tests so far:
1. `not_literal_string`, `multiplied_string`, `multiplied_literal_string`, `truncated_string_literals_become_literal_string`, `adding_string_literals_and_literal_string`, ...
In Rust's test there are formatting for variable in tests file like `y = "a".repeat(TypeInferenceBuilder::MAX_STRING_LITERAL_SIZE + 1)`. The question is: how do I correctly insert variable in tests in mdtest the same way?

2. `from_import_with_no_module_name` and `exception_handler_with_invalid_syntax`
````
NOTE: Parsing diagnostic's error is not resolving, also already tried using full message
```py
from import bar   # error: "Expected a module name"
reveal_type(bar)  # revealed: Unknown
```
````

3. All comprehension tests (and some functions), because variables should be infered inside it's scope. Don't know how to do it in mdtest. 

---

_Converted to draft by @Lexxxzy on 2024-10-12 10:46_

---

_Comment by @Lexxxzy on 2024-10-13 21:02_

Most of the work is done. Now I’m just waiting for feedback on what’s finished and looking for some help with the issues I’m facing before I can keep going.

---

_Marked ready for review by @Lexxxzy on 2024-10-13 21:04_

---

_Review requested from @carljm by @Lexxxzy on 2024-10-13 21:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/boolean.md`:37 on 2024-10-14 18:47_

Let's consolidate the stuff that's about the TODO to just the one line where it's relevant, that's not the main point of this test.
````suggestion
### Not function

```py
from typing import reveal_type

def f():
    return 1

a = not f
b = not reveal_type

reveal_type(a)  # revealed: Literal[False]
# TODO Unknown should not be part of the type of typing.reveal_type
# reveal_type(b)  # revealed: Literal[False]
````

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/boolean.md`:163 on 2024-10-14 21:35_

I'd like to break up this large file a bit, lets move out this whole section into a set of files, e.g. `comparison/integers.md`, `comparison/non_boolean_returns.md`, `comparison/strings.md`, `comparison/unsupported.md`. There's probably more that we could break out as well, but that's probably good enough for now.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/classes.md`:20 on 2024-10-14 21:38_

Let's generally hard-wrap lines at 80 columns in text, and also edit this text a bit for clarity/conciseness:
```suggestion
In type stubs, classes can reference themselves in their base class definitions. For example, in `typeshed`, we have `class str(Sequence[str]): ...`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/boolean.md`:259 on 2024-10-14 21:42_

nit: I'd rather put notes like this as Python comments right next to the relevant line
````suggestion
```py
def str_instance() -> str: ...
a = "abc" == "abc"
b = "ab_cd" <= "ab_ce"
c = "abc" in "ab cd"
d = "" not in "hello"
e = "--" is "--"
f = "A" is "B"
g = "--" is not "--"
h = "A" is not "B"
i = str_instance() < "..."
# ensure we're not comparing the interned salsa symbols, which compare by order of declaration.
j = "ab" < "ab_cd"
````

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/boolean.md`:286 on 2024-10-14 21:43_

Same here:
````suggestion
```py
a = 1 in 7      # error: "Operator `in` is not supported for types `Literal[1]` and `Literal[7]`"
b = 0 not in 10 # error: "Operator `not in` is not supported for types `Literal[0]` and `Literal[10]`"
c = object() < 5 # error: "Operator `<` is not supported for types `object` and `Literal[5]`"
d = 5 < object()

reveal_type(a)  # revealed: bool
reveal_type(b)  # revealed: bool
reveal_type(c)  # revealed: Unknown
# TODO should be `Unknown` but we don't yet check if `__lt__` signature is valid for right operand
reveal_type(d)  # revealed: bool
````

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/classes.md`:41 on 2024-10-14 21:44_

These two tests only incidentally use classes, they are really about testing declarations and shadowing. I'd put them in their own `shadowing.md` instead.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/classes.md`:59 on 2024-10-14 21:45_

I would put these tests into two separate files, `subscript/instance.md` (for the tests about `__getitem__`) and `subscript/class.md` (for the tests about `__class_getitem__`).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/classes.md`:195 on 2024-10-14 21:54_

Similar to above, let's localize the TODO stuff
````suggestion
### Class getitem non-class union

```py
flag = True

if flag:
    class Identity:
        def __class_getitem__(self, x: int) -> str:
            pass
else:
    Identity = 1

a = Identity[42] # error: "Cannot subscript object of type `Literal[Identity] | Literal[1]` with no `__getitem__` method"
# TODO should _probably_ emit `str | Unknown`
reveal_type(a) # revealed: Unknown 
```
````

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/classes.md`:197 on 2024-10-14 21:55_

Let's put this in `call/callable_instance.md`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/classes.md`:52 on 2024-10-14 21:56_

Let's avoid "This test ensures..." style boilerplate in favor of simple declarative statements of verified behavior.
```suggestion
No diagnostic is raised in the case of explicit shadowing:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/conditionals.md`:14 on 2024-10-14 21:58_

I don't think this really adds any clarity over just reading the test.

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/conditionals.md`:1 on 2024-10-14 21:59_

Similar to above cases, I'd rather break up into more smaller files in a nested directory structure. Here we could do `conditional/if_expression.md` and `conditional/if_statement.md`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/conditionals.md`:183 on 2024-10-14 22:14_

This test can go in `narrow/not_none.rs`.

And I don't think we need the prose sentence here, it doesn't add anything.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/conditionals.md`:192 on 2024-10-14 22:16_

The existence of `y` in this test was really a hacky workaround for the difficulty of asserting revealed types in locations other than end-of-scope in our previous test style. This is a simpler way to get the same intended test:
```suggestion
x = None if flag else 1
if x is not None:
    reveal_type(x)  # revealed: Literal[1]

reveal_type(x)  # revealed: None | Literal[1]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/conditionals.md`:207 on 2024-10-14 22:18_

Let's put this test in `narrow/match.rs`.

And some edits:
````suggestion
### Singleton pattern

```py
x = None if flag else 1
match x:
    case None:
        # TODO intersection simplification: should be just Literal[0] | None
        reveal_type(x)  # revealed: Literal[0] | None | Literal[1] & None
```
````

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exceptions.md`:53 on 2024-10-14 22:22_

````suggestion
## Dynamic exception types

```py
def foo(x: type[AttributeError], y: tuple[type[OSError], type[RuntimeError]], z: tuple[type[BaseException], ...]):
    try:
        w
    except x as e:
        # TODO should be `AttributeError`
        reveal_type(e)  # revealed: @Todo
    except y as f:
        # TODO should be `OSError | RuntimeError`
        reveal_type(f)  # revealed: @Todo
    except z as g:
        # TODO should be `BaseException`
        reveal_type(g)  # revealed: @Todo
````

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exceptions.md`:58 on 2024-10-14 22:24_

```suggestion
TODO(Alex): Once we support `sys.version_info` branches, we can set `--target-version=py311` in these tests and the inferred type will just be `BaseExceptionGroup`
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/functions.md`:52 on 2024-10-14 22:25_

This doesn't belong under Functions heading; let's put it in `call/constructor.md`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/functions.md`:3 on 2024-10-14 22:25_

Let's put these tests in `call/function.rs`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exceptions.md`:78 on 2024-10-14 22:26_

I'll leave the rest of these uncommented to reduce comment volume, but let's follow this kind of pattern everywhere instead:
````suggestion
## Except\* with specific exception

```py
try:
    x
except* OSError as e:
    # TODO(Alex): more precise would be `ExceptionGroup[OSError]`
    reveal_type(e)  # revealed: Unknown | BaseExceptionGroup
```
````

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/functions.md`:153 on 2024-10-14 22:28_

```suggestion
Parameter `x` of type `str` is shadowed and reassigned with a new `int` value inside the function. No diagnostics should be generated.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/loops.md`:1 on 2024-10-14 22:29_

Similar to some above cases, let's instead have a `loops/` directory with e.g. `for_loop.md`, `while_loop.md` files in it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/numbers.md`:15 on 2024-10-14 22:30_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/numbers.md`:32 on 2024-10-14 22:30_

```suggestion
We don't support big integer literals; we just infer `int` type instead:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/numbers.md`:41 on 2024-10-14 22:31_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/numbers.md`:68 on 2024-10-14 22:31_

I'd rather put these in `unary/integers.md` and `binary/integers.md` files

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/strings.md`:34 on 2024-10-14 22:32_

I'd rather put f-strings in their own file; it can either be a top-level `fstrings.md` or we can add a `literal/` subdir and put several of these files in there.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/strings.md`:79 on 2024-10-14 22:33_

These belong in `subscript/string.md`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/strings.md`:115 on 2024-10-14 22:33_

Bytes are a separate type from strings, this should go in `bytes.md` or `literal/bytes.md`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/variables.md`:55 on 2024-10-14 22:34_

All the tests from here onward should go in `shadowing.md` along with some other tests I commented above.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/variables.md`:13 on 2024-10-14 22:35_

These could go in `assignment.md`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/variables.md`:3 on 2024-10-14 22:35_

This test can go with some others in `conditional/if_statement.md`

---

_@carljm reviewed on 2024-10-14 22:40_

Thank you so much, this is a lot of good work! Excellent translation of the tests to the new assertion style.

A few general comments, which I commented partway through but not everywhere they occur:

- Avoid `TODO` in test titles, and keep TODO comments minimal in length and as close as possible to the specific line/assertion that isn't quite right yet.
- Avoid boilerplate phrases like "This test ensures that" or "Check that" or "We can infer the type when ..." or anything similar -- minimize the words that are not adding specific value to the test in question.
- Break up into more smaller files; I commented on this throughout to try to give a clear idea of an organization that I think would work well.

I appreciate your quick work on this PR and would like to get it landed ASAP to minimize conflicts as new tests are added in other PRs! If you won't have time to update it in the next day-ish, please just let me know, I'm also happy to make the updates and get it landed!

Thanks again.

---

_Comment by @Lexxxzy on 2024-10-14 23:50_

> would like to get it landed ASAP

All clear. Will try to push fixes as soon as possible

---

_Comment by @Lexxxzy on 2024-10-15 11:27_

@carljm 
> A few general comments, which I commented partway through but not everywhere they occur

I tried to follow the style you described. Please take a look at the tests with the new structure. Sorry for all the issues!


---

_Review requested from @carljm by @Lexxxzy on 2024-10-15 11:45_

---

_Comment by @sharkdp on 2024-10-15 13:28_

@Lexxxzy I already ported the two narrowing-related tests to Markdown while fixing a bug in the narrowing logic (https://github.com/astral-sh/ruff/pull/13758). Let me know if you need help resolving the conflicts here.

---

_Comment by @Lexxxzy on 2024-10-15 14:54_

@sharkdp Thanks for letting me know! I rebased on the current main, but somehow `subscript/string.md - Subscript on strings - Function return` test started to fail. Now it says that revealed type is `@Todo`, and not `str`. Can somebody check what went wrong? Maybe it is because of #5fa82f (Sync vendored typeshed stubs) commit?

---

_Comment by @Lexxxzy on 2024-10-15 14:58_

> Can somebody check what went wrong?

Yep, I checked, actually on @sharkdp pr merge commit (74bf4b) test is running fine, but on 5fa82f it started to fail

---

_Comment by @AlexWaygood on 2024-10-15 15:01_

> > Can somebody check what went wrong?
> 
> Yep, I checked, actually on @sharkdp pr merge commit (74bf4b) test is running fine, but on 5fa82f it started to fail

Yes -- unfortunately I had to change the assertion the test was making as part of that PR (https://github.com/astral-sh/ruff/pull/13753). This is because typeshed now declares `str.__getitem__` to be an overloaded function, and we don't understand overloaded functions yet.

---

_Comment by @AlexWaygood on 2024-10-15 15:02_

This is the specific commit in that PR where I tweaked the test @Lexxxzy: https://github.com/astral-sh/ruff/pull/13753/commits/8ec3b30ea480cd63c01c3ef54aed73517c343196. You'll need to change the assertion to `@Todo` in your `mdtest` port of the test as well.

Sorry for the bother!

---

_Comment by @Lexxxzy on 2024-10-15 15:08_

> You'll need to change the assertion to @Todo in your mdtest port of the test as well.

Ok, i see. Thanks for the info!

---

_@carljm approved on 2024-10-15 16:30_

This is great!! Thank you so much for the quick and thorough work. I'm going to make a few minor cosmetic tweaks, push those, wait for CI, and then merge.

---

_Merged by @carljm on 2024-10-15 18:23_

---

_Closed by @carljm on 2024-10-15 18:23_

---

_Comment by @AlexWaygood on 2024-10-15 19:09_

Thanks so much for taking this on @Lexxxzy! A big first contribution, and a really helpful one for us!!

---

_Comment by @Lexxxzy on 2024-10-15 19:41_

Big thanks @carljm for reviewing my first PR to ruff! Learned a lot about that project while porting tests

---
