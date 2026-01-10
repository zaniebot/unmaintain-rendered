```yaml
number: 14143
title: "Formatter undocumented deviation: extra unnecessary wrapping of arguments and extra parenthesis for long return types"
type: issue
state: open
author: Avasam
labels:
  - formatter
assignees: []
created_at: 2024-11-06T23:34:52Z
updated_at: 2024-11-07T08:24:59Z
url: https://github.com/astral-sh/ruff/issues/14143
synced_at: 2026-01-10T11:09:55Z
```

# Formatter undocumented deviation: extra unnecessary wrapping of arguments and extra parenthesis for long return types

---

_Issue opened by @Avasam on 2024-11-06 23:34_

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  undocumented deviation black return

This is similar to https://github.com/astral-sh/ruff/issues/11791, but for return type annotations rather than variable annotations.

* A minimal code snippet that reproduces the bug.

With `preview = True`:

Black:
```py
class Polygon:
    def cut_section(self, line) -> tuple[
        RegularPolygon | Polygon | Triangle | Point | Point2D | Point3D | Segment2D | Segment3D | Segment | None,
        RegularPolygon | Polygon | Triangle | Point | Point2D | Point3D | Segment2D | Segment3D | Segment | None,
    ]: ...
```

Ruff:
```py
class Polygon:
    def cut_section(
        self, line
    ) -> tuple[
        RegularPolygon | Polygon | Triangle | Point | Point2D | Point3D | Segment2D | Segment3D | Segment | None,
        RegularPolygon | Polygon | Triangle | Point | Point2D | Point3D | Segment2D | Segment3D | Segment | None,
    ]: ...
```

See https://github.com/microsoft/python-type-stubs/commit/9eb6bb50ec2d16eccde13e8d51fc03f11a7c0807 for a lot more examples

As an added note, with `preview = False`, I also get the following deviation:

Black:
```py
def get_basis_vectors() -> tuple[
    tuple[Literal[1], Literal[0], Literal[0]],
    tuple[Literal[0], Literal[1], Literal[0]],
    tuple[Literal[0], Literal[0], Literal[1]],
]: ...
```

Ruff:
```py
def get_basis_vectors() -> (
    tuple[
        tuple[Literal[1], Literal[0], Literal[0]],
        tuple[Literal[0], Literal[1], Literal[0]],
        tuple[Literal[0], Literal[0], Literal[1]],
    ]
): ...
```

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
`ruff format`

* The current Ruff settings (any relevant sections from your `pyproject.toml`).

```toml
[tool.ruff]
line-length = 130
# Target oldest supported Python version
target-version = "py39"
preview = true
```

* The current Ruff version (`ruff --version`).
`ruff 0.7.2`


---

_Label `formatter` added by @AlexWaygood on 2024-11-07 00:00_

---

_Comment by @MichaReiser on 2024-11-07 07:16_

Thanks. We're aware of the deviation with `preview=False` but the way this gets formatted with `preview=True` is rather bad

```py
class Polygon:
    def cut_section(
        self, line
    ) -> tuple[
        RegularPolygon
        | Polygon
        | Triangle
        | Point
        | Point2D
        | Point3D
        | Segment2D
        | Segment3D
        | Segment
        | None,
        RegularPolygon
        | Polygon
        | Triangle
        | Point
        | Point2D
        | Point3D
        | Segment2D
        | Segment3D
        | Segment
        | None,
    ]: ...

```

---

_Comment by @MichaReiser on 2024-11-07 08:24_

Interestingly, prettier does the exact same [playground](https://prettier.io/playground/#N4Igxg9gdgLgprEAuEAzArlMMCW0AEA+jAE4CGUAzqhCQLaFgAWFUcANgBQKkCeS+AILoAJjhgAZCAHMAorBK8ANPhFkYZAQHkADrmhl2AbQDKUCAHdU7MgGs4AXQCUAzgB0o+fLv1RDRgFUoPE8AH3wyACMwADoAcXQcdhEAYRYoNnZ8cK1IgCs4bDhiktKy8orK8ocPJ3xgDy8mnFR8TjUNfABeLvwAOWg4OobPJrH8EjgYdBJPAbZGsYBfRabmVg5u-B5FGOlE5L2pxnTMzhxYdvUyJydVr0np2fx1jM3QnPzCmHORLo6bh4VlAPBgsL4iKQKNRaAxXmcdvwhKJxFI5ApeC42osfCF-EEQtkItF4gdUqd3t4vkUqrS6XSalBhvd8C02gDur15kN6iyxo8ZnNBizgeN4Zteoi9mSjjAThsuBcfgDbiyBc9xVkPlSCthfv9rncoMCPKaoGDsISwOg5ZRviF3KNxvg7exUCp2Bc4PcsdMdOw4EY+V4AEpwfY2EgABQg7F40mgwaJMbjCZBTvG4QAKiQcBRpAGk+EY0qi-gS7AAEwAETLFZgAGZaxmxuETOG6Dwa2X29JO7Amz2Ozwy9yy2GI2Ro7H44mW01izO02Wc3moAXvfOvIvS1vkxAld29zuB83ndv8L3+zAj+eiVeeIPj5fh7BR8KMw56ib02aLRDrVte1oEdO9XVQH0BD9AMgz3Cd0EjFNZ3TO9F1TOdUPwVd80LZ96zrA8qzPc8T0bYjnTbV8b3IzMXz7R8aNbOjr3fBZn3gxClwwkjyy4lCeOw9dcMw-C8MI6iCKVJ9MIfIih3o095JY58x0-b8PBAJQQAgPQQkoZBQCnEhLCjKcEH0lBDAsMheH0rTInIMB7BgEwyE7CQvWQVBDDtezHOckwdDIMALmkZBSHQOAtLtOgcHCkhIq0uAAA8dDgXNr0MLN0r8XM4As7z2F8kBKFCgMAEV0AgeAvJ8qKQDyShkpMMq4Eq6q4Fqor6oARyq+Ao2MnQLJAMhKAAWjYOARGmzSQChJJQpSCA6DoMhkFG9h2Dm0qhLgQQYFIHBIhtOAo3Sjy2C64qmBgOh2AAdSYcR8qCsA4HbKhxBwAA3cReA2sBKDskAfsigBJKAZtgEwwFzPRBChkwYF4ANrvqnRjLtB7yB0DbMfy9Kfs6rSLjtEgYEGsg+3WpBCuKoKSHJjaUbSyg4ZwPQ5sxpUHpwEQYCYZAAA4AAYtMmPqcEmKmafRrSNEiPmBaFpBKy09A7SzKICrqpK6EiaaZpECR83Qam4AAMVhdRcHXDayBtCAQCWJYgA)

---
