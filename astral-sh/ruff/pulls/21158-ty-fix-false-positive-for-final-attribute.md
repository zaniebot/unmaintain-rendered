```yaml
number: 21158
title: "[ty] Fix false positive for Final attribute assignment in __init__"
type: pull_request
state: merged
author: saada
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-final-init-1409
created_at: 2025-10-31T02:48:53Z
updated_at: 2025-11-11T20:54:05Z
url: https://github.com/astral-sh/ruff/pull/21158
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Fix false positive for Final attribute assignment in __init__

---

_Pull request opened by @saada on 2025-10-31 02:48_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1409

This PR allows `Final` instance attributes to be initialized in `__init__` methods, as mandated by the Python typing specification (PEP 591). Previously, ty incorrectly prevented this initialization, causing false positive errors.

The fix checks if we're inside an `__init__` method before rejecting Final attribute assignments, allowing the first assignment during instance initialization while still preventing reassignment elsewhere.

## Test Plan

- Added new test coverage in `final.md` for the reported issue with `Self` annotations
- Updated existing tests that were incorrectly expecting errors 
- All 278 mdtest tests pass
- Manually tested with real-world code examples

This is my first contribution to the project, so I'd appreciate any feedback or guidance!

---

_Review requested from @carljm by @saada on 2025-10-31 02:48_

---

_Review requested from @AlexWaygood by @saada on 2025-10-31 02:48_

---

_Review requested from @sharkdp by @saada on 2025-10-31 02:48_

---

_Review requested from @dcreager by @saada on 2025-10-31 02:48_

---

_Comment by @github-actions[bot] on 2025-10-31 03:10_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-11 20:42:52.251115074 +0000
+++ new-output.txt	2025-11-11 20:42:55.645138741 +0000
@@ -864,10 +864,7 @@
 qualifiers_annotated.py:87:1: error[call-non-callable] Object of type `GenericAlias` is not callable
 qualifiers_annotated.py:88:1: error[call-non-callable] Object of type `GenericAlias` is not callable
 qualifiers_final_annotation.py:18:7: error[invalid-type-form] Type qualifier `typing.Final` expected exactly 1 argument, got 2
-qualifiers_final_annotation.py:52:9: error[invalid-assignment] Cannot assign to final attribute `ID4` on type `Self@__init__`
-qualifiers_final_annotation.py:54:9: error[invalid-assignment] Cannot assign to final attribute `ID5` on type `Self@__init__`
-qualifiers_final_annotation.py:57:13: error[invalid-assignment] Cannot assign to final attribute `ID6` on type `Self@__init__`
-qualifiers_final_annotation.py:59:13: error[invalid-assignment] Cannot assign to final attribute `ID6` on type `Self@__init__`
+qualifiers_final_annotation.py:54:9: error[invalid-assignment] Cannot assign to final attribute `ID5` in `__init__` because it already has a value at class level
 qualifiers_final_annotation.py:65:9: error[invalid-assignment] Cannot assign to final attribute `ID7` on type `Self@method1`
 qualifiers_final_annotation.py:71:1: error[invalid-assignment] Reassignment of `Final` symbol `RATE` is not allowed: Symbol later reassigned here
 qualifiers_final_annotation.py:81:1: error[invalid-assignment] Cannot assign to final attribute `DEFAULT_ID` on type `<class 'ClassB'>`
@@ -1006,5 +1003,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1008 diagnostics
+Found 1005 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @github-actions[bot] on 2025-10-31 03:12_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
schema_salad (https://github.com/common-workflow-language/schema_salad)
- schema_salad/metaschema.py:73:9: error[invalid-assignment] Cannot assign to final attribute `original_doc` on type `Self@__init__`
- schema_salad/metaschema.py:79:9: error[invalid-assignment] Cannot assign to final attribute `idx` on type `Self@__init__`
- schema_salad/metaschema.py:85:9: error[invalid-assignment] Cannot assign to final attribute `fileuri` on type `Self@__init__`
- schema_salad/metaschema.py:91:9: error[invalid-assignment] Cannot assign to final attribute `baseuri` on type `Self@__init__`
- schema_salad/metaschema.py:97:9: error[invalid-assignment] Cannot assign to final attribute `namespaces` on type `Self@__init__`
- schema_salad/metaschema.py:103:9: error[invalid-assignment] Cannot assign to final attribute `schemas` on type `Self@__init__`
- schema_salad/metaschema.py:109:9: error[invalid-assignment] Cannot assign to final attribute `addl_metadata` on type `Self@__init__`
- schema_salad/metaschema.py:115:9: error[invalid-assignment] Cannot assign to final attribute `imports` on type `Self@__init__`
- schema_salad/metaschema.py:121:9: error[invalid-assignment] Cannot assign to final attribute `includes` on type `Self@__init__`
- schema_salad/metaschema.py:127:9: error[invalid-assignment] Cannot assign to final attribute `no_link_check` on type `Self@__init__`
- schema_salad/metaschema.py:133:9: error[invalid-assignment] Cannot assign to final attribute `container` on type `Self@__init__`
- schema_salad/metaschema.py:150:9: error[invalid-assignment] Cannot assign to final attribute `fetcher` on type `Self@__init__`
- schema_salad/metaschema.py:152:9: error[invalid-assignment] Cannot assign to final attribute `cache` on type `Self@__init__`
- schema_salad/metaschema.py:163:9: error[invalid-assignment] Cannot assign to final attribute `vocab` on type `Self@__init__`
- schema_salad/metaschema.py:164:9: error[invalid-assignment] Cannot assign to final attribute `rvocab` on type `Self@__init__`
- schema_salad/python_codegen_support.py:70:9: error[invalid-assignment] Cannot assign to final attribute `original_doc` on type `Self@__init__`
- schema_salad/python_codegen_support.py:76:9: error[invalid-assignment] Cannot assign to final attribute `idx` on type `Self@__init__`
- schema_salad/python_codegen_support.py:82:9: error[invalid-assignment] Cannot assign to final attribute `fileuri` on type `Self@__init__`
- schema_salad/python_codegen_support.py:88:9: error[invalid-assignment] Cannot assign to final attribute `baseuri` on type `Self@__init__`
- schema_salad/python_codegen_support.py:94:9: error[invalid-assignment] Cannot assign to final attribute `namespaces` on type `Self@__init__`
- schema_salad/python_codegen_support.py:100:9: error[invalid-assignment] Cannot assign to final attribute `schemas` on type `Self@__init__`
- schema_salad/python_codegen_support.py:106:9: error[invalid-assignment] Cannot assign to final attribute `addl_metadata` on type `Self@__init__`
- schema_salad/python_codegen_support.py:112:9: error[invalid-assignment] Cannot assign to final attribute `imports` on type `Self@__init__`
- schema_salad/python_codegen_support.py:118:9: error[invalid-assignment] Cannot assign to final attribute `includes` on type `Self@__init__`
- schema_salad/python_codegen_support.py:124:9: error[invalid-assignment] Cannot assign to final attribute `no_link_check` on type `Self@__init__`
- schema_salad/python_codegen_support.py:130:9: error[invalid-assignment] Cannot assign to final attribute `container` on type `Self@__init__`
- schema_salad/python_codegen_support.py:147:9: error[invalid-assignment] Cannot assign to final attribute `fetcher` on type `Self@__init__`
- schema_salad/python_codegen_support.py:149:9: error[invalid-assignment] Cannot assign to final attribute `cache` on type `Self@__init__`
- schema_salad/python_codegen_support.py:160:9: error[invalid-assignment] Cannot assign to final attribute `vocab` on type `Self@__init__`
- schema_salad/python_codegen_support.py:161:9: error[invalid-assignment] Cannot assign to final attribute `rvocab` on type `Self@__init__`
- Found 195 diagnostics
+ Found 165 diagnostics

mypy (https://github.com/python/mypy)
- mypy/types.py:554:9: error[invalid-assignment] Cannot assign to final attribute `raw_id` on type `Self@__init__`
- mypy/typestate.py:106:9: error[invalid-assignment] Cannot assign to final attribute `_subtype_caches` on type `Self@__init__`
- mypy/typestate.py:107:9: error[invalid-assignment] Cannot assign to final attribute `_negative_subtype_caches` on type `Self@__init__`
- mypy/typestate.py:109:9: error[invalid-assignment] Cannot assign to final attribute `_attempted_protocols` on type `Self@__init__`
- mypy/typestate.py:110:9: error[invalid-assignment] Cannot assign to final attribute `_checked_against_members` on type `Self@__init__`
- mypy/typestate.py:111:9: error[invalid-assignment] Cannot assign to final attribute `_rechecked_types` on type `Self@__init__`
- mypy/typestate.py:112:9: error[invalid-assignment] Cannot assign to final attribute `_assuming` on type `Self@__init__`
- mypy/typestate.py:113:9: error[invalid-assignment] Cannot assign to final attribute `_assuming_proper` on type `Self@__init__`
- mypy/typestate.py:114:9: error[invalid-assignment] Cannot assign to final attribute `inferring` on type `Self@__init__`
- Found 1712 diagnostics
+ Found 1703 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/threadingtools.py:214:9: error[invalid-assignment] Cannot assign to final attribute `upstream2downstream` on type `Self@__init__`
- hydpy/core/threadingtools.py:215:9: error[invalid-assignment] Cannot assign to final attribute `starters` on type `Self@__init__`
- hydpy/core/threadingtools.py:216:9: error[invalid-assignment] Cannot assign to final attribute `dependencies` on type `Self@__init__`
- Found 665 diagnostics
+ Found 662 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@sharkdp reviewed on 2025-10-31 10:52_

Thank you for working on this. I think we should probably just allow the *first* assignment to a `Final`-qualified attribute in `__init__`? It would also be good to add some tests that assignments to those attributes from other methods is not allowed. 

---

_Label `ty` added by @AlexWaygood on 2025-10-31 10:54_

---

_Comment by @AlexWaygood on 2025-10-31 13:42_

Thank you for the PR!!

Some other tests that I think we're currently missing: this should probably be disallowed. It looks like we already _do_ disallow it, but I can't find any explicit tests for it (@sharkdp, please correct me if I'm wrong!):

```py
from typing import Final

class Foo:
    X: Final = 42

    def __init__(self):
        self.X = 56
```

This should probably be allowed, and it would be great to add a test for it:

```py
import sys
from typing import Final

```py
class Bar:
    X: Final[int]

    def __init__(self):
        if sys.version_info >= (3, 11):
            self.X = 42
        else:
            self.X = 56
```

---

_Comment by @saada on 2025-10-31 14:47_

Thanks for the feedback. After several iterations, I think this is the cleanest approach.

The Rust fix works by modifying the `invalid_assignment_to_final` closure to check whether we're currently inside an `__init__` method before rejecting Final attribute assignments. Before the fix, ty was rejecting *all* assignments to Final attributes regardless of context, which caused false positives for valid Python code like `self.x: Final[int]` followed by `self.x = 1` in `__init__`. Per PEP 591, Final instance attributes must be initialized exactly once, and `__init__` is the designated place for that initialization. The fix adds an `is_in_init()` helper that checks `current_function_definition().name.id == "__init__"`, and only rejects Final assignments when this check is false. 

---

_Comment by @carljm on 2025-10-31 16:20_

We should confirm our assumptions about the desired behavior here against the [conformance tests](https://github.com/python/typing/blob/main/conformance/tests/qualifiers_final_annotation.py) and the [spec](https://typing.python.org/en/latest/spec/qualifiers.html#uppercase-final) and verify that the behavior we're implementing matches them. It's also useful to validate against other type checkers.

> I think we should probably just allow the _first_ assignment to a `Final`-qualified attribute in `__init__`?

The conformance suite does not require this. It does require that we allow multiple conditional assignments in `__init__` (and not only in cases where one of them is unreachable):

```py
class C:
    x: Final[int]

    def __init__(self, cond: bool):
        if cond:
            self.x = 1
        else:
            self.x = 2
```

It appears that both [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=8cc45115516c8cbea885131d730e413b) and [pyright](https://pyright-play.net/?pyrightVersion=1.1.405&strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoBiqAhgDYCwAUFQManEDODUAwgFxVRdQAebhJUgG1UMALpVO3ACYBTYFAD6i1EhjKAFA1mlgASg6VuxqNt0A6HlAC8UAIxSTpncEs2oAJipA) simply always allow multiple assignments in `__init__`. I think it's fine if we do that, too.

The other thing highlighted by the conformance suite results is that we should still error on assignment in `__init__` if a value was already assigned at class level.

---

_Comment by @saada on 2025-10-31 16:32_

I checked the other type checkers and here's a summary of what was found

Our implementation matches closer to pyright!

   | Case | Description | ty | pyright | mypy | Winner |
   |------|-------------|-------|---------|------|--------|
   | **Conditional assignments in __init__** | `if cond: self.x=1` `else: self.x=2` | ✅ Allow | ✅ Allow | ❌ Error | ty & pyright ✓ |
   | **Reassign class-level Final in __init__** | Class has `x: Final = 10`, reassign in `__init__` | ❌ Error | ❌ Error | ✅ Allow | ty & pyright ✓ |
   | **Assignment in helper method** | Called from `__init__` but different method | ❌ Error | ❌ Error | ✅ Allow | ty & pyright ✓ |
   | **Nested conditionals** | Multiple if/else levels in `__init__` | ✅ Allow | ✅ Allow | ❌ Error | ty & pyright ✓ |
   | **Try/except assignments** | Assign in try and except blocks | ✅ Allow | ✅ Allow | ❌ Error | ty & pyright ✓ |
   | **Uninitialized Final** | `x: Final[int]` with no `__init__` or value | ✅ Allow | ❌ Error | ❌ Error | **pyright & mypy ✓** |
   | **Assignment after super()** | Assign before and after `super().__init__()` | ✅ Allow | ✅ Allow | ❌ Error | ty & pyright ✓ |
   | **Bare Final with value** | `x: Final = 1`, reassign in `__init__` | ❌ Error | ❌ Error | ✅ Allow | ty & pyright ✓ |
   
   
  We might have to take a stance here since there's a gap in behavior between the tools

---

_Comment by @carljm on 2025-10-31 17:39_

@saada Thanks for generating the chart. I think the chart is not really accurate, though, so I'm wondering what method you used to generate it.

For example, I checked the first row, and [mypy does allow the two conditional assignments](https://mypy-play.net/?mypy=latest&python=3.12&gist=167ed2f02ccbc97b8c32cd8bb773f314), so I'm not sure why it's marked with an X in that row on your chart. In fact, as I mentioned in my previous comment, [mypy allows two subsequent assignments in `__init__` even if they are not conditional at all](https://mypy-play.net/?mypy=latest&python=3.12&gist=8cc45115516c8cbea885131d730e413b).

And in the second row, you claim that pyright is OK with assignment in `__init__` if there is a class-level value assigned, but that [doesn't seem to be true either](https://pyright-play.net/?pyrightVersion=1.1.405&strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoBiqAhgDYCwAUFQManEDODUAwgFxVRdQAebhJUgG1UMALpQAvFACMABiqduAEwCmwKAH1NqJDG0AKBqtLAAlB0rdrUY6YB0PKbKpA).

In general I think the behavior of mypy and pyright is much more consistent with each other in this area than your chart suggests, and where they agree that's a pretty strong signal that we should also agree, unless we have very good reason to do otherwise.

---

_Comment by @saada on 2025-10-31 23:12_

@carljm Thank you for the detailed review and pointing me to the conformance suite! After thorough investigation, I can confirm our implementation is correct.

## Key Finding: Mypy Playground Mystery Solved

Your mypy playground link shows no error because it uses **untyped parameters**:
```python
def __init__(self, cond):  # ← No type annotation
```

When I test [with typed parameters](https://mypy-play.net/?mypy=latest&python=3.12&gist=8c198505b1775b2c1aa9fcb90e686c2f), mypy errors (as documented in the conformance suite):
```python
def __init__(self, cond: bool):  # ← With type annotation
# mypy ERROR: Cannot assign to final attribute
```

## Comprehensive Comparison

### Case 1: Sequential Assignments in `__init__`
```python
class C:
    x: Final[int]
    def __init__(self):
        self.x = 1
        self.x = 2
```

**Playground links**: [mypy ✅](https://mypy-play.net/?mypy=latest&python=3.12&gist=8cc45115516c8cbea885131d730e413b) | [pyright ✅](https://pyright-play.net/?pyrightVersion=1.1.405&strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoBiqAhgDYCwAUFQManEDODUAwgFxVRdQAebhJUgG1UMALpVO3ACYBTYFAD6i1EhjKAFA1mlgASg6VuxqNt0A6HlAC8UAIxSTpncEs2oAJipA)

---

### Case 2: Conditional Assignments (untyped)
```python
class C:
    x: Final[int]
    def __init__(self, cond):  # ← NO type annotation
        if cond:
            self.x = 1
        else:
            self.x = 2
```

**Playground links**: [mypy ✅](https://mypy-play.net/?mypy=latest&python=3.12&gist=167ed2f02ccbc97b8c32cd8bb773f314) | [pyright ✅](https://pyright-play.net/?pyrightVersion=1.1.405&strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoBiqAhgDYBQ5AxqcQM51QDCAXOVB1AB4uEmkBtVDAC67TgBMApsCgB9OaiQwFACjpTSwADRQqYFBICUvAMSxEU3kjQpcU8Z05JZ%2Bw2yefOGrQDouUAC8UACMjp6aGh5enj7A-kFQAExAA)

**Note**: Mypy skips checking untyped function bodies by default. Pyright shows parameter type warnings but no Final assignment error.

---

### Case 3: Conditional Assignments (typed)
```python
class C:
    x: Final[int]
    def __init__(self, cond: bool):  # ← WITH type annotation
        if cond:
            self.x = 1
        else:
            self.x = 2
```

**Playground links**: [mypy ❌](https://mypy-play.net/?mypy=latest&python=3.12&gist=8c198505b1775b2c1aa9fcb90e686c2f) | [pyright ✅](https://pyright-play.net/?pyrightVersion=1.1.405&strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoBiqAhgDYBQ5AxqcQM51QDCAXOVB1AB4uEmkBtVDAC67TgBMApsCgB9OaiQwFACjpTSwADRQqYFBN4AjMGFIBKNpxuZZ%2Bw9dvONWgHRcoAXigBGcc6aGk7ONq7AHt5QAExAA)

**Conformance**: Lines 57-59 in `qualifiers_final_annotation.py` have NO `# E` marker → must allow

---

### Case 4: Class-Level Value Reassignment ⚠️
```python
class C:
    x: Final[int] = 10  # ← Has value at class level
    def __init__(self):
        self.x = 20
```

**Playground links**: [mypy ✅ WRONG](https://mypy-play.net/?mypy=latest&python=3.12&gist=50554d824cfb6b4e1a6245f5f8ac13d7) | [pyright ❌](https://pyright-play.net/?pyrightVersion=1.1.405&strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoBiqAhgDYBQ5AxqcQM51QDCAXOVB1AB4uEmkBtVDAC6UALxQAjAAZ2nACYBTYFAD6a1EhgaAFHSWlgASjadzUA0YB0XCVABMMoA)

**Conformance**: Line 54 in `qualifiers_final_annotation.py` has `# E: Already initialized` → must error

---

## Summary Table

| Test Case              | mypy    | pyright | ty  | Conformance |
|------------------------|---------|---------|-----|-------------|
| Sequential in __init__ | ✅       | ✅       | ✅   | Must allow  |
| Conditional (untyped)  | ✅       | ✅       | ✅   | Must allow  |
| Conditional (typed)    | ❌ ERROR | ✅       | ✅   | Must allow  |
| Class-level value      | ✅ WRONG | ❌       | ❌   | Must error  |
| Outside __init__       | ❌       | ❌       | ❌   | Must error  |

**Result: ty matches pyright perfectly (5/5 conformant)**

## Official Conformance Results

From the `python/typing` repository:

**pyright**: `conformant = "Pass"` ✅

**mypy**: `conformant = "Partial"` with note:
> "Does not allow conditional assignment of Final instance variable in __init__ method."

Our implementation is correct as-is and matches the conformance suite requirements.

---

**Note**: Our test suite (`final.md`) has been updated to explicitly cover both typed and untyped conditional assignment cases to ensure comprehensive coverage of all scenarios discussed above.


---

_Comment by @carljm on 2025-11-04 20:37_

Aha! Thank you for pointing out that my mypy result was due to not having any type annotations on the `__init__` method. This always trips me up!

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:91 on 2025-11-04 20:39_

I don't think we need to comment on every non-error, unless there's something notable or unusual about it.
```suggestion
        self.FINAL_E = 1
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:187 on 2025-11-04 20:39_

```suggestion
        self.INSTANCE_FINAL_C = 1
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:282 on 2025-11-04 20:39_

```suggestion
        self.LEGAL_I = 1
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:394 on 2025-11-04 20:39_

```suggestion
        self.DEFINED_IN_INIT = 1
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:3657 on 2025-11-04 21:02_

I think this logic currently leads to false negatives in cases like these:

```py
from typing import Final

class C:
    x: Final[int] = 100

def _(c: C):
    c.x = 1

def __init__(c: C):
    c.x = 1

class A:
    def __init__(self, c: C):
        c.x = 1
```

We should add these cases to the tests, and expand our checking here to validate that we are in the `__init__` method of the class on which we are trying to assign a final attribute. Even then, there is still this case to consider as well:

```py
from typing import Final, Self

class C:
    x: Final[int]

    def __init__(self, c: Self):
        c.x = 3
```

Ideally this should not be allowed either; `__init__` should only be allowed to mutate the `self` object (the first argument). I think this should be fairly easy to check, I believe we have some other cases where we already check this.

---

_@carljm reviewed on 2025-11-04 21:02_

Thanks for working on this! I agree with your conclusions above about the correct semantics.

---

_Comment by @saada on 2025-11-06 23:11_

Thank you for the thorough feedback! How does this look?

---

_Comment by @carljm on 2025-11-11 20:42_

Looks good, thank you! I pushed a few simplifications, will merge once CI is green.

---

_Merged by @carljm on 2025-11-11 20:54_

---

_Closed by @carljm on 2025-11-11 20:54_

---
