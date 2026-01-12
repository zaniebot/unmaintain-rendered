```yaml
number: 18905
title: "`PLW1641` false positives for dataclasses"
type: issue
state: open
author: lengau
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-06-23T22:14:10Z
updated_at: 2025-07-08T07:24:52Z
url: https://github.com/astral-sh/ruff/issues/18905
synced_at: 2026-01-12T15:54:56Z
```

# `PLW1641` false positives for dataclasses

---

_@lengau_

### Summary

When using `dataclasses` or other decorators that generate the `__hash__` method for a class, if you implement an `__eq__` method manually PLW1641 emits an error.

The following script shows that the classes are correctly hashable, but ruff does not detect the `__hash__` method.

```python
import dataclasses

@dataclasses.dataclass(frozen=True)
class TrueNegative:

    a: str
    b: int


@dataclasses.dataclass(repr=True, eq=False, frozen=False, unsafe_hash=False)
class TruePositive:

    a: str
    b: int

    def __eq__(self, other):
        return True


@dataclasses.dataclass(frozen=True)  # Implicit eq=True, generates a __hash__
class FalsePositive:

    a: str
    b: int

    def __eq__(self, other):
        return True


@dataclasses.dataclass(eq=True, frozen=True)  # Explicit eq=True, generates a __hash__
class FalsePositive1:

    a: str
    b: int

    def __eq__(self, other):
        return True


@dataclasses.dataclass(unsafe_hash=True)  # Always generates a __hash__
class FalsePositive2:

    a: str
    b: int

    def __eq__(self, other):
        return True



for cls in [TrueNegative, FalsePositive, FalsePositive1, FalsePositive2]:
    assert hash(cls("a", 1)) is not None

for cls in [TruePositive]:
    assert cls("a", 1).__hash__ is None
```

### Version

0.12.0

---

_Comment by @ntBre on 2025-06-23 22:30_

Nice, thanks for all of the examples! I think that pretty much covers the cases described in the [docs](https://docs.python.org/3/library/dataclasses.html#:~:text=Here%20are%20the%20rules%20governing%20implicit%20creation%20of%20a%20__hash__()%20method.%20Note%20that%20you%20cannot%20both%20have%20an%20explicit%20__hash__()%20method%20in%20your%20dataclass%20and%20set%20unsafe_hash%3DTrue%3B%20this%20will%20result%20in%20a%20TypeError.).

I think it makes sense to update the rule like this, but I'm certainly open to other thoughts.

---

_Label `rule` added by @ntBre on 2025-06-23 22:30_

---

_Label `needs-decision` added by @ntBre on 2025-06-23 22:30_

---

_Comment by @squeaky-pl on 2025-07-01 09:39_

I also hit this when updating to 0.12.

Code is similar to this

```python
@dataclasses.dataclass(frozen=True)
class A:
    a: int

    def __eq__(self, other: object) -> bool:
        match other:
            case A():
                return self.a == other.a
            case B():
                return self.a == other.b
            case _:
                return False
```

---

_Comment by @antonagestam on 2025-07-06 15:30_

This also applies to Pydantic models, and perhaps likely also to other similar libraries.

---

_Comment by @MichaReiser on 2025-07-08 07:24_

I'm sort of surprised that `dataclass` generates a hash function for

```py
@dataclasses.dataclass(frozen=True)  # Implicit eq=True, generates a __hash__
class FalsePositive:

    a: str
    b: int

    def __eq__(self, other):
        return True
```

because the generated hash function violates the contract between eq and hash where `a == b` implies that `hash(a) == hash(b)`. For the given example, all instances are equal and would require that all instances should map to the same hash value.

However, we should update the documentation if this reasoning makes sense to broaden why it's recommended to override both. 

---
