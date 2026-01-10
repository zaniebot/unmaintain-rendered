```yaml
number: 20425
title: "RUF028: `fmt: on/off` comments  around class signature not flagged as invalid"
type: issue
state: open
author: Xophmeister
labels:
  - bug
  - rule
assignees: []
created_at: 2025-09-15T20:55:51Z
updated_at: 2025-09-16T06:47:55Z
url: https://github.com/astral-sh/ruff/issues/20425
synced_at: 2026-01-10T11:09:59Z
```

# RUF028: `fmt: on/off` comments  around class signature not flagged as invalid

---

_Issue opened by @Xophmeister on 2025-09-15 20:55_

### Summary

If you put `fmt: off` and `fmt: on` pragmata around a class signature, the `fmt: on` reset pragma doesn't seem to be followed and the rest of the module is not formatted.

For example:

https://play.ruff.rs/0a9e3f0a-29a4-4774-81ab-fdeec065e636

The above is a minimal example, but I was using this to try to make my generics easier to read:

```python
# fmt: off
class ConcreteThingy(ThingyABC[
    Path,    # SourceT
    str,     # MetadataT
    SHA256,  # HashT
]):
# fmt: on

    # Nothing from this point is formatted...
    ...

# ...even outside the class definition
```

If you try putting the pragmata deeper into the tree -- say into the subclass parentheses, or the generics parentheses -- then the `fmt: off` pragma is completely ignored. For example:

Before:
```python
class ConcreteThingy(
    ThingyABC[
        # fmt: off
        Path,    # SourceT
        str,     # MetadataT
        SHA256,  # HashT
        # fmt: on
    ]
): ...
```

After:
```python
class ConcreteThingy(
    ThingyABC[
        # fmt: off
        Path,  # SourceT
        str,  # MetadataT
        SHA256,  # HashT
        # fmt: on
    ]
): ...
```

### Version

ruff 0.11.10

---

_Label `suppression` added by @ntBre on 2025-09-15 21:12_

---

_Label `formatter` added by @ntBre on 2025-09-15 21:12_

---

_Comment by @ntBre on 2025-09-15 21:17_

Thanks for the report! It looks like we also don't flag [invalid-formatter-suppression-comment (RUF028)](https://docs.astral.sh/ruff/rules/invalid-formatter-suppression-comment/#invalid-formatter-suppression-comment-ruf028) anywhere here. 

[playground](https://play.ruff.rs/5b335c3c-f4d0-4e01-a1a4-16585333d871)

I found a somewhat related issue in https://github.com/astral-sh/ruff/issues/9875, and it looks like the suggestion to use `fmt: skip` instead may also help here:

```py
class ConcreteThingy(
    ThingyABC[
        Path,    # SourceT
        str,     # MetadataT
        SHA256,  # HashT
    ]
): # fmt: skip
    ...
```

---

_Comment by @MichaReiser on 2025-09-16 06:47_

The formatter behavior is intentional. `fmt: off`/`fmt: on` comments are only allowed between statements:

> As such, adding # fmt: on and # fmt: off comments within expressions will have no effect. In the following example, both list entries will be formatted, despite the # fmt: off:
>
> https://docs.astral.sh/ruff/formatter/#format-suppression

For your use case, the recommended approach is to use `fmt: skip` as shown by @ntBre 

That RUF028 doesn't flag the invalid suppression comment is a bug.



---

_Renamed from "`fmt: on/off` pragmata around a class signature don't work properly" to "RUF028: `fmt: on/off` comments  around class signature not flagged as invalid" by @MichaReiser on 2025-09-16 06:47_

---

_Label `suppression` removed by @MichaReiser on 2025-09-16 06:47_

---

_Label `formatter` removed by @MichaReiser on 2025-09-16 06:47_

---

_Label `bug` added by @MichaReiser on 2025-09-16 06:47_

---

_Label `rule` added by @MichaReiser on 2025-09-16 06:47_

---
