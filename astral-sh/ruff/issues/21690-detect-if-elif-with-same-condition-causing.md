```yaml
number: 21690
title: "Detect `if-elif` with same condition causing unreachable branch"
type: issue
state: open
author: injust
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-11-29T10:05:10Z
updated_at: 2025-11-30T12:02:21Z
url: https://github.com/astral-sh/ruff/issues/21690
synced_at: 2026-01-10T11:10:00Z
```

# Detect `if-elif` with same condition causing unreachable branch

---

_Issue opened by @injust on 2025-11-29 10:05_

### Summary

This isn't caught by type checking, so it'd be nice if Ruff could handle this.

```python
def test(foo: str) -> None:
    if foo == "bar":
        print(1)
    elif foo == "bar":  # Unreachable
        print(2)
```

---

There's an edge case if the body of the if/elif is identical:

```python
def test(foo: str) -> None:
    if foo == "bar":
        pass
    elif foo == "bar":
        pass
```

https://play.ruff.rs/f190be4e-f165-4dfa-9756-a7f6ce6d2ae7

Currently, SIM114 auto-fixes this to:

```python
def test(foo: str) -> None:
    if foo == "bar" or foo == "bar":
        pass
```

And PLR1714's offered fix is:

```python
def test(foo: str) -> None:
    if foo in {"bar", "bar"}:
        pass
```

Then B033 auto-fixes it to:

```python
def test(foo: str) -> None:
    if foo in {"bar"}:
        pass
```

Which is...okay but not great? Instead of providing fixes to merge the branches and all, it seems better to flag it as unreachable and let the user figure out what they want. It might be a typo they need to fix, or just a duplicate branch they want to delete entirely.

---

Another edge case is `if-return-elif` with the same conditional:

```python
def test(foo: str) -> None:
    if foo:
        return
    elif foo:
        pass
```

https://play.ruff.rs/dff0d4ea-05dd-4bdb-bba4-ece0ba703e6f

Pyright detects the `elif` branch is unreachable, however RET505 provides this auto-fix:

```python
def test(foo: str) -> None:
    if foo:
        return
    if foo:
        pass
```

Which makes it slightly harder to understand _why_ it's unreachable (`if-elif` with the same conditional is much easier to grok than reading through the `if` body and understanding that it always returns).

---

_Renamed from "Detect if-elif with same condition causing unreachable branch" to "Detect `if-elif` with same condition causing unreachable branch" by @injust on 2025-11-29 10:22_

---

_Comment by @MichaReiser on 2025-11-30 12:02_

ty could detect this if `foo` is a union over a given set of string literals. 

However, it fails to detect the `elif` as unreachable because it can't narrow foo to not being bare because of subclassing (what if `foo` is a subclass of `str` with some weird `eq` implementation). But it should be able to detect this if you change the type annotation of `foo` to `LiteralString` (related discussion https://play.ty.dev/37a754f0-0971-4c91-a2e4-1047ce0077a5)

We could add some very naive narrowing to Ruff that detects a very limited set of cases but I'm inclined to fold this into a more generic unreachable rule.

Related to https://github.com/astral-sh/ruff/issues/18013

---

_Label `rule` added by @MichaReiser on 2025-11-30 12:02_

---

_Label `needs-decision` added by @MichaReiser on 2025-11-30 12:02_

---
