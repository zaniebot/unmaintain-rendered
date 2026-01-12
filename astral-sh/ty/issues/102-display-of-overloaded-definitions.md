```yaml
number: 102
title: Display of overloaded definitions
type: issue
state: open
author: dhruvmanila
labels:
  - diagnostics
  - needs-design
  - overloads
assignees: []
created_at: 2025-05-01T20:36:33Z
updated_at: 2026-01-12T08:34:43Z
url: https://github.com/astral-sh/ty/issues/102
synced_at: 2026-01-12T15:54:22Z
```

# Display of overloaded definitions

---

_@dhruvmanila_

Currently, an overloaded definition is displayed using the following format:

```py
Overload[<signature of overload 1>, <signature of overload 2>, ...]
```

For example:

```py
Overload[() -> None, (x: int) -> int]
```

The main use cases are `reveal_type`, error messages (?), hover implementation in the language server.

This issue is to keep track of whether we want to improve the display and if so then how?

There was some discussion over at https://github.com/astral-sh/ruff/pull/17294#issuecomment-2789998693.

---

_Renamed from "[red-knot] Display of overloaded definitions" to "Display of overloaded definitions" by @MichaReiser on 2025-05-07 15:24_

---

_Label `overloads` added by @AlexWaygood on 2025-05-10 18:02_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-11 07:40_

---

_Label `needs-design` added by @dhruvmanila on 2025-07-11 15:48_

---

_Comment by @erictraut on 2025-07-11 23:11_

Related to the question about _how_ overloads should be displayed is the question about _when_ they should be displayed.

In pyright, we try to avoid showing overloads when the user hovers over a call expression. In that context, the language server can typically tell which overload signature matches the supplied arguments, so it's possible to display only the overload that applies in that context. We added this behavior in response to strong user feedback that seeing all of the overloads was often overwhelming. You might want to consider doing something similar in ty. 

There will still be situations (e.g. outside of a call expression or in cases where the overload matching algorithm matches more than one overload signature) where it will still be necessary to display multiple overload signatures, but those cases should be relatively rare.

---

_Comment by @InSyncWithFoo on 2025-07-12 01:06_

Regarding [#19275](https://github.com/astral-sh/ruff/pull/19275), here's my proposal:

* For bound methods: The format for a single overload is the same as that of a single-signature bound method. A note is added to help avoiding user confusion (compare with the existing `bound method C[int].f(v: str) -> str`). It can be either invalid syntax (`(bound method)`) or a comment (`# bound method`); I have no strong opinion on this.

	```python
	(bound method)
	@overload
	def C[int].f(v: str) -> str: ...
	@overload
	def C[int].f(v: int) -> int: ...
	```	

* For literal functions/unbound methods: The format for a single overload is the same as that of a single-signature function/unbound method.

	```python
	@overload
	def f(v: str) -> str: ...
	@overload
	def f(v: int) -> int: ...
	```

* For (anonymous) callables: The format for a single overload is similar to that of a single-signature function/unbound method, but the name is replaced with `<anonymous>`, which is emphatically not a valid identifier.

	```python
	@overload
	def <anonymous>(v: str) -> str: ...
	@overload
	def <anonymous>(v: int) -> int: ...
	```

Note that, since this "prettified" format is multiline, it is only used in hover popups and not type checker messages. Here's how an union would be displayed if this format were to be used indiscriminately:

```python
int | Literal['foo'] | @overload
def f(v: str) -> str: ...
@overload
def f(v: int) -> int: ... | type[C]
```

One important thing to note is that the `: ...` parts, while do not carry useful information on their own, might be necessary for highlighters to work correctly. Take GitHub's highlighter for an example:

* With bodies: Everything is highlighted nicely.

	```python
	@overload
	def f(v: str) -> str: ...
	@overload
	def f(v: int) -> int: ...
	```

* Without bodies: The second `@overload` and `def` pair are not highlighted.

	```python
	@overload
	def f(v: str) -> str
	@overload
	def f(v: int) -> int
	```

This format is largely influenced by Pyright's, but with `@overload`s added to serve as some kind of visual separator and indicator. No strong opinion on that either.

---

_Comment by @MichaReiser on 2025-07-12 16:32_

Thanks for writing this up. I'll try to take a look on Monday. 

To clarify my understanding, is this what pyright would show:

```py
def f(v: str) -> str
def f(v: int) -> int
```

---

_Comment by @InSyncWithFoo on 2025-07-12 18:02_

@MichaReiser Pyright would show something like this, to be exact:

```python
(function)
def f(v: str) -> str: ...
def f(v: int) -> int: ...
```

---

_Comment by @MichaReiser on 2025-07-28 07:42_

I like the more compact representation and, for now, i'd probably defual to whatever pylance does unless we have good reasons not to,

---

_Comment by @InSyncWithFoo on 2025-07-28 08:07_

@MichaReiser In that case, here's a snippet for reference ([playground](https://pyright-play.net/?pyrightVersion=1.1.403&pythonVersion=3.14&strict=true&enableExperimentalFeatures=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMpgBuApiADZgCGAJgLABQDDAxmZQM7tQDCA2gCoBdAFwMo4qNWLAoAfVmokMeQAp2xMsAA0UQsKj8dAegCUUALQA%2BKADkwKYvoB0LpvQlQAAkVIUaYiSkZYDUNbV19VBgzK0wUGGdXdwlvEnIqOmTxIKgQ9U0dPSh2GBAY6xKQRKc3Dxy8sMLI%2BKgAH2LS8rj8dsrqtwZU3wyGepUiqK6o-voh9P96MaLKrr6oFxrGRelc8eaejrKLayi2w5m3HLQVE1Esrx95zLqdkIn4qfiZjzm-Z8DXntDqtShd7kt9mcVsdulDQeskgFxCBiDAAK4gFC5Ab0WRQAC8PBUAAYTE5gAwAMRQABCYDRKGoAFlUQALMDUXh8KKCHQAeTSf14oU0%2BgAymFPNwmt1PjAdCLgOLJdKIsCYZVBIIqVSoCoIGyOSZdRDZTDpgjNtTTdDYmsNm48YTuOTdQLhjReAJBIr9HwhDLJub4jpvb6eN6ZbaKqVtfRKbr9YbqMb45JAfklRGAKooADWKDAAHcULy1UHYhaHWmxpm-bxcwXi6Wo50NfDqwwnRGea60%2B6nl6hOH-WX3tFg-KoGG6xGA2ro4c4wm00mYOyUyaM2F6zzAx9JzNrdvRT34mP9Iv7YjcQTsf3BRlheO5Qrlm27bGdavgAzmDAkHsVNj2CIEKxOL5LS3UD3yOT8qigrZu2uYCoAHIUwIPSsQz1WCQRAZdE0ISgQCQSgACMyGIVDTXA7oj3TGDLw-GMEOrIA)):

<details>

```python
class C[T]:
    def __init__(self, v: T, /) -> None: ...

    @overload
    def f(self, v: int) -> int: ...
    @overload
    def f(self, v: str) -> str: ...

    def f(self, v: int | str) -> int | str: ...


@overload
def f(v: int) -> int: ...
@overload
def f(v: str) -> str: ...

def f(v: int | str) -> int | str: ...


def g():
    @overload
    def f(v: int) -> int: ...
    @overload
    def f(v: str) -> str: ...

    def f(v: int | str) -> int | str: ...

    return f
```

```python
_ = C(0).f
# BoundMethod[C[int], Overload[(self: Self@C, v: int) -> int, (self: Self@C, v: str) -> str]]
#
# (method)
# def f(v: int) -> int: ...
# def f(v: str) -> str: ...

_ = C.f
# Overload[[T](self: C[T], v: int) -> int, [T](self: C[T], v: str) -> str]
#
# (method)
# def f(self: C[Unknown], v: int) -> int: ...
# def f(self: C[Unknown], v: str) -> str: ...

_ = C[int].f
# Overload[[T](self: C[T], v: int) -> int, [T](self: C[T], v: str) -> str]
#
# (method)
# def f(self: C[int], v: int) -> int: ...
# def f(self: C[int], v: str) -> str: ...

_ = f
# Overload[(v: int) -> int, (v: str) -> str]
#
# (function)
# def f(v: int) -> int: ...
# def f(v: str) -> str: ...

_ = g()
# Overload[(v: int) -> int, (v: str) -> str]
#
# (variable)
# def f(v: int) -> int: ...
# def f(v: str) -> str: ...
```
</details>

As you can see, Pyright doesn't use the notation `C[...].f` to denote a bound method like ty would. It also doesn't have an "anonymous" overloaded callable type (or at least I couldn't find out how to make one). Would you mind expanding on that?

---

_Comment by @carljm on 2025-08-05 02:35_

Here's [an example of an anonymous overloaded callable type in pyright](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAK4MYAxmADYA0UYAbgKYgVgCGAJgLABQPpFrAM6CoAWThEwJABSSS5CgEoAXDyjqoAAXpMWHNRvYNgUCAxgALMO2mCGFYDQAeyzChiKoAWgB8bmK4AdMEG6tqMzGxc3BpQRiZmlta29o5QLlCCMCCevpnZQSExhsam5lY2dg7Orqj4AD75Od5%2BdVCNWSCFgTy93PFQAPrSCK7icmAqoVAgDIysFIPwCAwjgYkVikA)

---

_Comment by @InSyncWithFoo on 2025-08-05 03:20_

@carljm That one still has a name: `method`.

---

_Comment by @carljm on 2025-08-05 03:24_

@InSyncWithFoo the name `method` doesn't appear anywhere in pyright's display of the type.

And in principle, it shouldn't -- the protocol can be satisfied by any object with an attribute called `method` that is callable -- it doesn't have to be a function defined with the name `method`. In type system terms, the type can only be known to be some kind of callable with the given overloaded signature.

---

_Comment by @InSyncWithFoo on 2025-08-05 03:41_

@carljm The name is displayed in the hover popup, not the "reveal type" diagnostic (unless I'm terribly mistaken, I think we are discussing the prettified format). Also, we might not want to display `method` as a callable attribute (i.e., `method: Overload[...]`); it just doesn't look particularly pretty or readable that way.

---

_Comment by @carljm on 2025-08-05 03:52_

Ah, sorry, missed the distinction between the prettified hover output and the inline diagnostic output.

There may not be any way to get an anonymous overloaded callable type in pyright.

---

_Comment by @asukaminato0721 on 2026-01-08 15:48_

for hover popup, pyrefly only show the matched overload, zuban current show all of them.


---

_Comment by @dhruvmanila on 2026-01-12 08:34_

> for hover popup, pyrefly only show the matched overload, zuban current show all of them.

ty does the same: https://play.ty.dev/2269ac45-edf2-45fe-88a3-7674e502b83d

---
