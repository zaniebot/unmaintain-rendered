```yaml
number: 17618
title: "[red-knot] Update call binding to return all matching overloads"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/overload-call-evaluation-1
created_at: 2025-04-24T22:38:52Z
updated_at: 2025-04-30T20:03:23Z
url: https://github.com/astral-sh/ruff/pull/17618
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Update call binding to return all matching overloads

---

_@dhruvmanila_

## Summary

This PR updates the existing overload matching methods to return an iterator of all the matched overloads instead.

This would be useful once the overload call evaluation algorithm is implemented which should provide an accurate picture of all the matched overloads. The return type would then be picked from either the only matched overload or the first overload from the ones that are matched.

In an earlier version of this PR, it tried to check if using an intersection of return types from the matched overload would help reduce the false positives but that's not enough. [This comment](https://github.com/astral-sh/ruff/pull/17618#issuecomment-2842891696) keep the ecosystem analysis for that change for prosperity.

> [!NOTE]
>
> The best way to review this PR is by hiding the whitespace changes because there are two instances where a large match expression is indented to be inside a loop over matching overlods
>
> <img width="1207" alt="Screenshot 2025-04-28 at 15 12 16" src="https://github.com/user-attachments/assets/e06cbfa4-04fa-435f-84ef-4e5c3c5626d1" />

## Test Plan

Make sure existing test cases are unaffected and no ecosystem changes.

---

_Label `red-knot` added by @dhruvmanila on 2025-04-24 22:38_

---

_Comment by @github-actions[bot] on 2025-04-24 22:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Closed by @dhruvmanila on 2025-04-28 20:15_

---

_Reopened by @dhruvmanila on 2025-04-28 20:15_

---

_Comment by @dhruvmanila on 2025-04-28 20:29_

I'm marking this as ready for review mainly to ask for feedback on the changes and the effect it has on the ecosystem changes. The PR description has my ecosystem analysis and it does reveal that this change would remove ~300 false positives by "hiding" it via intersection or `Never` type.

---

_Marked ready for review by @dhruvmanila on 2025-04-28 20:29_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-28 20:29_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-28 20:29_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-28 20:29_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-28 20:29_

---

_@carljm reviewed on 2025-04-29 23:18_

Thank you, that's a super-thorough analysis of the ecosystem impacts!

My main conclusion from looking at it is that it's hard to evaluate at this point. In particular, there are a lot of overloads in typeshed defined with protocol arguments, and so we are evaluating a lot of overloads as matching that really shouldn't be.

But I am seeing enough cases that would give counter-intuitive or wrong behavior (due to people relying on being able to silence an overlapping-overload error and count on the type checker picking the first matching overload) that I'm pretty convinced we will need to implement the full specced overload matching algorithm, in order to achieve sufficiently compatible behavior with the ecosystem. So I don't think we should merge this as-is.

I do think that in order to implement the full matching algorithm, we will still need to check all overloads and preserve all bindings, not short-circuit, and it seems like that was the bulk of the code changes here? So I think we should modify the `return_type` method (where the intersection is currently implemented) to instead return the return type of the first matching overload, with a TODO to implement proper overload matching, and then land this?

---

_Converted to draft by @dhruvmanila on 2025-04-30 16:15_

---

_Comment by @dhruvmanila on 2025-04-30 18:14_

<details><summary>(Archived) Ecosystem analysis for an <a href="https://github.com/astral-sh/ruff/pull/17618/commits/49286d6c4869b73ad2c4d423342d2333fbabdcc6">earlier version of this PR</a>:</summary>
<p>

## Ecosystem analysis

### Overall stats

```
                           Diagnostic Analysis Report                           
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━┳━━━━━━━━━┳━━━━━━━┳━━━━━━━━━━━━┓
┃ Diagnostic ID                      ┃ Severity ┃ Removed ┃ Added ┃ Net Change ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━╇━━━━━━━━━╇━━━━━━━╇━━━━━━━━━━━━┩
│ lint:call-non-callable             │ error    │      27 │     0 │        -27 │
│ lint:call-possibly-unbound-method  │ warning  │      21 │    15 │         -6 │
│ lint:division-by-zero              │ error    │       0 │    13 │        +13 │
│ lint:invalid-argument-type         │ error    │      96 │     1 │        -95 │
│ lint:invalid-assignment            │ error    │      22 │     1 │        -21 │
│ lint:invalid-base                  │ error    │       2 │     2 │          0 │
│ lint:invalid-return-type           │ error    │      56 │     1 │        -55 │
│ lint:invalid-super-argument        │ error    │       1 │     1 │          0 │
│ lint:invalid-type-form             │ error    │       2 │     0 │         -2 │
│ lint:no-matching-overload          │ error    │       5 │     0 │         -5 │
│ lint:non-subscriptable             │ error    │       6 │     0 │         -6 │
│ lint:not-iterable                  │ error    │       3 │     0 │         -3 │
│ lint:possibly-unbound-attribute    │ warning  │     124 │    84 │        -40 │
│ lint:possibly-unresolved-reference │ warning  │       3 │     0 │         -3 │
│ lint:type-assertion-failure        │ error    │      21 │    25 │         +4 │
│ lint:unresolved-attribute          │ error    │      50 │     0 │        -50 │
│ lint:unsupported-operator          │ error    │      30 │     4 │        -26 │
│ lint:unused-ignore-comment         │ warning  │       0 │     3 │         +3 │
├────────────────────────────────────┼──────────┼─────────┼───────┼────────────┤
│ TOTAL                              │          │     469 │   150 │       -319 │
└────────────────────────────────────┴──────────┴─────────┴───────┴────────────┘
Analysis complete. Found 18 unique diagnostic IDs.
Total diagnostics removed: 469
Total diagnostics added: 150
Net change: -319
```

After filtering out the diagnostics where it’s only the messages that have been updated due to the fact that the return type will now include additional types because of intersections, we get the following stats:

```
                           Diagnostic Analysis Report                           
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━┳━━━━━━━━━┳━━━━━━━┳━━━━━━━━━━━━┓
┃ Diagnostic ID                      ┃ Severity ┃ Removed ┃ Added ┃ Net Change ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━╇━━━━━━━━━╇━━━━━━━╇━━━━━━━━━━━━┩
│ lint:call-non-callable             │ error    │      27 │     0 │        -27 │
│ lint:call-possibly-unbound-method  │ warning  │       6 │     0 │         -6 │
│ lint:division-by-zero              │ error    │       0 │    13 │        +13 │
│ lint:invalid-argument-type         │ error    │      95 │     0 │        -95 │
│ lint:invalid-assignment            │ error    │      22 │     1 │        -21 │
│ lint:invalid-return-type           │ error    │      55 │     0 │        -55 │
│ lint:invalid-type-form             │ error    │       2 │     0 │         -2 │
│ lint:no-matching-overload          │ error    │       5 │     0 │         -5 │
│ lint:non-subscriptable             │ error    │       6 │     0 │         -6 │
│ lint:not-iterable                  │ error    │       3 │     0 │         -3 │
│ lint:possibly-unbound-attribute    │ warning  │      44 │     4 │        -40 │
│ lint:possibly-unresolved-reference │ warning  │       3 │     0 │         -3 │
│ lint:type-assertion-failure        │ error    │       0 │     4 │         +4 │
│ lint:unresolved-attribute          │ error    │      50 │     0 │        -50 │
│ lint:unsupported-operator          │ error    │      26 │     0 │        -26 │
│ lint:unused-ignore-comment         │ warning  │       0 │     3 │         +3 │
├────────────────────────────────────┼──────────┼─────────┼───────┼────────────┤
│ TOTAL                              │          │     344 │    25 │       -319 │
└────────────────────────────────────┴──────────┴─────────┴───────┴────────────┘
Analysis complete. Found 16 unique diagnostic IDs.
Total diagnostics removed: 344
Total diagnostics added: 25
Net change: -319
```

### Notes

The most common false positives which have been removed are around the usages of `getattr`, `dict.get`, and `open`.

Removed false positives:

```python
import urllib
import webbrowser

# Red knot matches against both overloads - `Literal[b""] & @Todo`
url = urllib.parse.urlunparse(("file", "", "", "", "", ""))
# The following false positive is removed
# red-knot: Argument to this function is incorrect: Expected `str`, found `Literal[b""]` [lint:invalid-argument-type]
webbrowser.open(url)
```

Diagnostic messages are getting verbose because of this change because the return type would be an intersection of all the return types from the matched overloads:

```diff
- Attribute `name` on type `Binance | None` is possibly unbound
+ Attribute `name` on type `(Binance & Bitpanda & Bitcoinde & Bitfinex & Bitmex & Bitstamp & Bybit & Coinbase & Coinbaseprime & Gemini & Htx & Iconomi & Independentreserve & Kraken & Okx & Poloniex & Woo & Kucoin) | None` is possibly unbound
```

Similarly, the return type of `open` is now:

```python
fd = open("file.text", "rb")

# revealed: `TextIOWrapper & BufferedRandom & BufferedWriter & BufferedReader & @Todo(specialized non-generic class)`
# revealed on main: `TextIOWrapper`
reveal_type(fd)
```

The revealed type for dictionary’s `get` method which leads to lots of diagnostics removed:

```python
data = {}
value = data.get("foo", None)

# revealed on main: `@Todo(Support for `typing.TypeVar` instances in type expressions) | None`
# revealed: `@Todo(Support for `typing.TypeVar` instances in type expressions) | (None & @Todo(Support for `typing.TypeVar` instances in type expressions))`
reveal_type(value)
```

### False negatives

False negative when `getattr` is involved:

```python
from typing import reveal_type

class Foo:
    pass

value = getattr(Foo, "foo", None)
if value is None:
	# red-knot does not report that `value` is not callable
	# revealed:         `Any & None`
	# revealed on main: `(Any & None) | None`
    value()
else:
    value()

# OR

def _(attr: str) -> int:
    # red-knot (main): Return type does not match returned value: Expected `int`, found `Any | None` [lint:invalid-return-type]
    # No error when using intersection of callables
    return getattr(Foo, attr, None)
```

Another false negative for `subprocess.check_output` when the output of that function requires it to be `str` but `bytes & Any` would not raise a diagnostic in that case. For example:

```python
import subprocess

def _(cmd: str) -> str:
	out = subprocess.check_output(cmd)
	
	# revealed: bytes & Any
	# revealed on main: bytes
	reveal_type(out)
	
	# This raises `invalid-return-type` diagnostic on main
	return out
```

### False positives

The `sum` return type now includes a `Literal[0]` which results in `division-by-zero` diagnostic if it’s used in that context:

```python
values = []

out = sum(values)

# revealed: `(int & @Todo(Support for `typing.TypeVar` instances in type expressions)) | Literal[0]`
# revealed on main: `int`
reveal_type(out)

# false positive: division-by-zero
1 / out
```

### `Never`

I’m seeing a bunch of `Never` instead of the actual types which might be coming from the fact that those types are disjoint which results in `Never`

```python
import os.path

x = os.path.join("a", "b")
# revealed on main: LiteralString
# revealed: Never
reveal_type(x)
```

Another case for the above:

```python
fd = open("file.txt", "rb")

# revealed: `Never`
# revealed on main: `str` (incorrect because it should be `bytes`)
reveal_type(fd.read())
```

This is because `join` return type is `str & bytes & LiteralString` and `bytes` is disjoint from `LiteralString` which results in a `Never` type.

An example from our mdtest for the descriptor protocol:

```python
from typing_extensions import Literal, LiteralString, overload

class Descriptor:
    @overload
    def __get__(self, instance: None, owner: type, /) -> Literal["called on class object"]: ...
    @overload
    def __get__(self, instance: object, owner: type | None = None, /) -> Literal["called on instance"]: ...
    def __get__(self, instance, owner=None, /) -> LiteralString:
        if instance:
            return "called on instance"
        else:
            return "called on class object"

class C:
    d: Descriptor = Descriptor()

# revealed on main: Literal["called on class object"]
# revealed: Never
reveal_type(C.d)

reveal_type(C().d)  # revealed: Literal["called on instance"]
```

The reason this is happening is because when the `instance` parameter is `None` when the field is accessed on the class directly, and it is assignable to both the overloads as `None` is assignable to both `None` and `object`:

```python
from knot_extensions import static_assert, is_assignable_to

static_assert(is_assignable_to(None, None))
static_assert(is_assignable_to(None, object)
```

And, both `Literal` types are disjoint from each other, so the intersection of them creates a `Never` type.

One way to solve this would be to use the return type of the first matched overload if the intersection type results into `Never` (done in https://github.com/astral-sh/ruff/pull/17618/commits/ce6d959ca27c0bf67e256e122399cb42a738eb6a and reverted) i.e., retain the old behavior if the return type is `Never` but this creates an issue when the `Never` would be beneficial. For example:

```python
from typing import overload

from knot_extensions import Unknown

@overload
def fix_name(name: None) -> None: ...
@overload
def fix_name(name: str) -> str: ...
def fix_name(name):
    return name

def _(foo: Unknown) -> str:
	# Here, we don't know the type of `foo`, so it matches both overloads but
	# their intersection is `Never` and so we would return `None` instead. But,
	# that's not assignable to the return type of the function (`str`) when the
	# `Never` type would be beneficial here
    return fix_name(foo)
```



</p>
</details> 

---

_Renamed from "[red-knot] Intersection of callables for evaluating an overload call" to "[red-knot] Update call binding to return all matched overloads" by @dhruvmanila on 2025-04-30 18:18_

---

_Marked ready for review by @dhruvmanila on 2025-04-30 18:18_

---

_Renamed from "[red-knot] Update call binding to return all matched overloads" to "[red-knot] Update call binding to return all matching overloads" by @dhruvmanila on 2025-04-30 18:19_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-30 18:20_

---

_@carljm approved on 2025-04-30 18:55_

Looks good!

---

_Merged by @dhruvmanila on 2025-04-30 20:03_

---

_Closed by @dhruvmanila on 2025-04-30 20:03_

---

_Branch deleted on 2025-04-30 20:03_

---
