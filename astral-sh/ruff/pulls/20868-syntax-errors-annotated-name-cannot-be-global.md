```yaml
number: 20868
title: "[syntax-errors] Annotated name cannot be global"
type: pull_request
state: merged
author: 11happy
labels:
  - rule
assignees: []
merged: true
base: main
head: annotated_name_cannot_be_global
created_at: 2025-10-14T16:57:22Z
updated_at: 2025-12-17T13:39:47Z
url: https://github.com/astral-sh/ruff/pull/20868
synced_at: 2026-01-12T15:57:11Z
```

# [syntax-errors] Annotated name cannot be global

---

_@11happy_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR implements a new semantic syntax error where annotated name can't be global
example
```
x: int = 1

def f():
    global x
    x: str = "foo"  # SyntaxError: annotated name 'x' can't be global
 ```

## Test Plan

<!-- How was it tested? -->
I have written tests as directed in #17412


---

_Review requested from @MichaReiser by @11happy on 2025-10-14 16:57_

---

_Review requested from @dhruvmanila by @11happy on 2025-10-14 16:57_

---

_Comment by @github-actions[bot] on 2025-10-14 17:19_


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

_@amyreese reviewed on 2025-10-14 19:01_

---

_Review comment by @amyreese on `crates/ruff_linter/src/linter.rs`:1080 on 2025-10-14 19:01_

Should we also add a test case like:

```
d: int = 1
def f3():
    global d
    d: str
```

to ensure we catch annotations without explicit assignments, and:

```
e: int = 1
def f3():
    e: str = "whatever"
```

to prove that this doesn't trigger on shadowed variables that aren't actually marked with `global`?

---

_Renamed from "[syntax-errors]: annotated name connot be global" to "[syntax-errors]: annotated name cannot be global" by @amyreese on 2025-10-14 19:35_

---

_@11happy reviewed on 2025-10-15 16:21_

---

_Review comment by @11happy on `crates/ruff_linter/src/linter.rs`:1080 on 2025-10-15 16:21_

yes, these are great additions to the testcases, I have added them, Thank you : )


---

_@amyreese approved on 2025-10-16 00:25_

---

_Review requested from @ntBre by @amyreese on 2025-10-16 00:25_

---

_@ntBre approved on 2025-10-17 20:26_

Thank you!

---

_Comment by @ntBre on 2025-10-17 20:35_

Actually, I thought of one more edge case, and it looks like we have a false positive:

```py
global f
f: int
```

This is actually allowed in module scope, for whatever reason, but the opposite order is not:

```py
f: int
global f
```

```shell
$ python <<EOF
global f
f: int
EOF
$ python <<EOF
f: int
global f
EOF
  File "<stdin>", line 2
SyntaxError: annotated name 'f' can't be global
```

---

_@ntBre requested changes on 2025-12-04 14:37_

Thanks for adding the new test case, but as I mentioned in my previous comment, this case is actually allowed by CPython, so the new snapshot is showing a false positive. We'll need to fix that and the merge conflicts before this can land.

---

_Comment by @11happy on 2025-12-05 07:28_

> Thanks for adding the new test case, but as I mentioned in my previous comment, this case is actually allowed by CPython, so the new snapshot is showing a false positive. We'll need to fix that and the merge conflicts before this can land.

Done, Thank you for the patience with this PR
: )

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:303 on 2025-12-08 19:59_

I pushed another test case that should still raise an error in the module scope. This `in_function_scope` check is too restrictive, we still miss the new case I added:

```shell
❯ python <<EOF
g: int = 1
global g
EOF
  File "<stdin>", line 2
SyntaxError: annotated name 'g' can't be global
```

I also noticed that ty actually has a false positive on the `f` case: https://play.ty.dev/d6285858-e5cf-429e-a983-291a243db217.

So we may want to fix that as well, if we can find a nice solution on the Ruff side.

For Ruff, we may be able to compare the range returned by `ctx.global` to the range of the `ExprName`. I think that's what we do for the other `global` error:

https://github.com/astral-sh/ruff/blob/e8540d9b085980bef22cc8e1d4c6e75531cf140c/crates/ruff_python_parser/src/semantic_errors.rs#L858-L860

---

_@ntBre requested changes on 2025-12-08 20:01_

Thanks for your continued work here!

I think we're still not quite getting the right answer in the module scope, but I had an idea for how we could fix it.

---

_@11happy reviewed on 2025-12-14 08:24_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:303 on 2025-12-14 08:24_

with the current fix, all tests are working as expected. however, I wanted to ask about an edge case:

does order matter in function scope for the `AnnotatedGlobal` error? Currently, Ruff emits `AnnotatedGlobal` for this case:
```python
a: str = "hel"
def foo():
    a: str = "foo"
    global a
```

But ty emits a different error  https://play.ty.dev/dc80ad75-cb1c-4f30-99f2-4fa555b191c7

also, since most of the errors in #17412 are now addressed, I would appreciate any suggestions for related issues or areas I could work on next. I would also be grateful if you could review some of my other open PRs when you have time. 
Thank you : )

---

_@ntBre reviewed on 2025-12-16 20:11_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:303 on 2025-12-16 20:11_

No I don't believe order matters in functions, only in the module scope:

```pycon
>>> def foo():
...     a: str
...     global a
...
  File "<python-input-1>", line 3
    global a
    ^^^^^^^^
SyntaxError: annotated name 'a' can't be global
>>> def bar():
...     global a
...     a: str
...
  File "<python-input-2>", line 3
    a: str
    ^^^^^^
SyntaxError: annotated name 'a' can't be global
```

at least the module scope is the only strange case I've found so far.

> But ty emits a different error [play.ty.dev/dc80ad75-cb1c-4f30-99f2-4fa555b191c7](https://play.ty.dev/dc80ad75-cb1c-4f30-99f2-4fa555b191c7)

I think we can file a follow-up issue for ty, but I would expect the fix to be here:

https://github.com/astral-sh/ruff/blob/0f373603ebd007196af4f211e14d421024a6b947/crates/ty_python_semantic/src/semantic_index/builder.rs#L2275-L2282

Assuming both errors can be emitted in this part of the code, I would probably prioritize the `AnnotatedGlobal` error here instead of `LoadBeforeGlobalDeclaration` to match CPython.

> also, since most of the errors in https://github.com/astral-sh/ruff/issues/17412 are now addressed, I would appreciate any suggestions for related issues or areas I could work on next. 

In general, anything with the `help wanted` or `bug` labels would be great :) We don't have too many big tracking issue like #17412 open at the moment. A couple of issues that I've opened that came to mind are https://github.com/astral-sh/ruff/issues/15642 and https://github.com/astral-sh/ruff/issues/17203 if those look interesting to you.

> I would also be grateful if you could review some of my other open PRs when you have time.

Will do! They're all in my inbox, just been busy with other tasks recently.

---

_@ntBre reviewed on 2025-12-16 20:13_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:305 on 2025-12-16 20:13_

Could you check how CPython and your code handle the class scope? I think we probably want to check `!ctx.in_module_scope()` here instead of specifically checking `in_functon_scope`. I'm picturing an example like this:

```py
class C:
    x: str
    global x

class D:
    global x
    x: str
```

It looks like these should both be errors:

```pycon
>>> class C:
...     x: str
...     global x
...
  File "<python-input-3>", line 3
    global x
    ^^^^^^^^
SyntaxError: annotated name 'x' can't be global
>>> class D:
...     global x
...     x: str
...
  File "<python-input-4>", line 3
    x: str
    ^^^^^^
SyntaxError: annotated name 'x' can't be global
```

---

_@11happy reviewed on 2025-12-17 09:41_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:305 on 2025-12-17 09:41_

done , Yes, these were earlier not being detected, I have updated the check with `!ctx.in_module_scope()` .

---

_Label `rule` added by @ntBre on 2025-12-17 13:36_

---

_@ntBre approved on 2025-12-17 13:38_

Thank you!

---

_Renamed from "[syntax-errors]: annotated name cannot be global" to "[syntax-errors] Annotated name cannot be global" by @ntBre on 2025-12-17 13:39_

---

_Merged by @ntBre on 2025-12-17 13:39_

---

_Closed by @ntBre on 2025-12-17 13:39_

---
