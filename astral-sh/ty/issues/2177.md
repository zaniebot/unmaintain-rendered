---
number: 2177
title: "ty: assert membership doesn't narrow Literal for TypedDict value"
type: issue
state: closed
author: dsfaccini
labels: []
assignees: []
created_at: 2025-12-23T02:15:02Z
updated_at: 2025-12-23T02:18:24Z
url: https://github.com/astral-sh/ty/issues/2177
synced_at: 2026-01-10T01:52:52Z
---

# ty: assert membership doesn't narrow Literal for TypedDict value

---

_Issue opened by @dsfaccini on 2025-12-23 02:15_

### Summary
Ty rejects a TypedDict literal value even after an `assert` narrowing check.

NOTE: playground link incoming

### Repro (self-contained)
`ty_repro_openai_audio.py`:
```python
# /// script
# dependencies = [
#     "openai>=1.57",
# ]
# ///

from typing import Literal
from dataclasses import dataclass

from openai.types.chat.chat_completion_content_part_input_audio_param import InputAudio


@dataclass
class Item:
    data: str
    format: str


def build_audio(item: Item) -> InputAudio:
    # Guard intended to narrow format to the allowed literal set.
    assert item.format in ("wav", "mp3")
    return InputAudio(data=item.data, format=item.format)


def main() -> Literal["ok"]:
    build_audio(Item(data="d", format="wav"))
    return "ok"


if __name__ == "__main__":
    print(main())
```

Run:
```bash
uvx ty check ty_repro_openai_audio.py
```

### Actual result
```
error[invalid-argument-type]: Invalid argument to key "format" with declared type `Literal["wav", "mp3"]` on TypedDict `InputAudio`
  --> ty_repro_openai_audio.py:22:12
   |
20 |     # Guard intended to narrow format to the allowed literal set.
21 |     assert item.format in ("wav", "mp3")
22 |     return InputAudio(data=item.data, format=item.format)
   |            ----------                 -------^^^^^^^^^^^
   |            |                          |      |
   |            |                          |      value of type `str`
   |            |                          key has declared type `Literal["wav", "mp3"]`
   |            TypedDict `InputAudio`
```

### Expected result
The `assert item.format in ("wav", "mp3")` should narrow `item.format` to the literal union and allow the `InputAudio` TypedDict assignment with no diagnostic.

### Notes
Ty version: 0.0.4 via `uvx ty check`.


---

_Comment by @dsfaccini on 2025-12-23 02:18_

closing this in favour of #2178 bc the openai dep is not necessary for repro. sorry for the spam.

---

_Closed by @dsfaccini on 2025-12-23 02:18_

---
