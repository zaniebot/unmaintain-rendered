```yaml
number: 2178
title: "ty: assert membership doesn't narrow `str` to `Literal` for `TypedDict` key (minimal repro)"
type: issue
state: closed
author: dsfaccini
labels: []
assignees: []
created_at: 2025-12-23T02:16:35Z
updated_at: 2025-12-23T02:35:25Z
url: https://github.com/astral-sh/ty/issues/2178
synced_at: 2026-01-12T15:54:26Z
```

# ty: assert membership doesn't narrow `str` to `Literal` for `TypedDict` key (minimal repro)

---

_@dsfaccini_

### Summary
Minimal repro of `assert` not narrowing a `str` to a `Literal` before populating a `TypedDict`.

Playground link: https://play.ty.dev/28bae877-4f82-452c-95c9-5cf5ba8cd0e2

### Repro (no external deps)
`ty_repro_assert_literal.py`:
```python
from typing import Literal, TypedDict
from dataclasses import dataclass

class InputAudio(TypedDict):
    data: str
    format: Literal['wav', 'mp3']

@dataclass
class Item:
    data: str
    format: str

def build_audio(item: Item) -> InputAudio:
    assert item.format in ('wav', 'mp3')
    return InputAudio(data=item.data, format=item.format)

if __name__ == '__main__':
    print(build_audio(Item('x', 'wav')))
```

Run:
```bash
uvx ty check ty_repro_assert_literal.py
```

### Actual result
```
error[invalid-argument-type]: Invalid argument to key "format" with declared type `Literal["wav", "mp3"]` on TypedDict `InputAudio`
  --> ty_repro_assert_literal.py:15:12
   |
13 | def build_audio(item: Item) -> InputAudio:
14 |     assert item.format in ('wav', 'mp3')
15 |     return InputAudio(data=item.data, format=item.format)
   |            ----------                 -------^^^^^^^^^^^
   |            |                          |      |
   |            |                          |      value of type `str`
   |            |                          key has declared type `Literal["wav", "mp3"]`
   |            TypedDict `InputAudio`
```

### Expected result
The membership assert should narrow `item.format` to `Literal['wav', 'mp3']`, allowing assignment into the `TypedDict` without diagnostics.

### Notes
Ty 0.0.4 via `uvx ty check`.


---

_Renamed from "ty: assert membership doesn't narrow str to Literal for TypedDict key (minimal repro)" to "ty: assert membership doesn't narrow `str` to `Literal` for `TypedDict` key (minimal repro)" by @dsfaccini on 2025-12-23 02:17_

---

_Comment by @MeGaGiGaGon on 2025-12-23 02:25_

This is the same as #1566

---

_Comment by @dsfaccini on 2025-12-23 02:35_

aahh sorry for that, I wasn't too diligent in my search, I'll close this one too then.

---

_Closed by @dsfaccini on 2025-12-23 02:35_

---
