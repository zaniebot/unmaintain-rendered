---
number: 2139
title: ty reports unknown-argument for Hugging Face tool choice dataclasses
type: issue
state: closed
author: dsfaccini
labels:
  - dataclasses
  - library
assignees: []
created_at: 2025-12-21T04:34:35Z
updated_at: 2026-01-06T11:29:07Z
url: https://github.com/astral-sh/ty/issues/2139
synced_at: 2026-01-10T01:51:14Z
---

# ty reports unknown-argument for Hugging Face tool choice dataclasses

---

_Issue opened by @dsfaccini on 2025-12-21 04:34_

### Summary
Ty reports `unknown-argument` for valid fields on Hugging Face chat tool choice dataclasses.

NOTE: I can't reproduce this in the playground when trying to write the example without the hugging face dependency https://play.ty.dev/0bb1b12d-1ea7-449b-b6cf-18c20f21027f

Nonetheless I do get the error when I use the hugging face package, so this may be something to bring up with the hugging face team

<img width="1548" height="482" alt="Image" src="https://github.com/user-attachments/assets/1fe7d9b2-de68-445a-953c-a9312facabda" />

### Repro (self-contained)
`ty_repro_hf.py`:
```python
# /// script
# dependencies = [
#     "huggingface_hub>=0.26",
# ]
# ///

from typing import Literal
from huggingface_hub import ChatCompletionInputFunctionName, ChatCompletionInputToolChoiceClass


def build_tool_choice(name: str) -> ChatCompletionInputToolChoiceClass:
    return ChatCompletionInputToolChoiceClass(
        function=ChatCompletionInputFunctionName(name=name)
    )


def main() -> Literal["ok"]:
    build_tool_choice("foo")
    return "ok"


if __name__ == "__main__":
    print(main())
```

Run:
```bash
uvx ty check ty_repro_hf.py
```

### Actual result
```
error[unknown-argument]: Argument `function` does not match any known parameter
  --> ty_repro_hf.py:13:9
...
error[unknown-argument]: Argument `name` does not match any known parameter
  --> ty_repro_hf.py:13:50
...
```

### Expected result
No diagnostics: the generated HF dataclasses expose `function` and `name` (see `huggingface_hub/inference/_generated/types/chat_completion.py` in the installed package).

### Notes
Looks like ty isn't recognizing the dataclass fields on these generated classes.


---

_Label `dataclasses` added by @sharkdp on 2025-12-21 08:26_

---

_Label `library` added by @sharkdp on 2025-12-21 08:26_

---

_Comment by @charliermarsh on 2025-12-21 13:37_

I think Pyright has the same behavior here, at least:

```
/Users/crmarsh/workspace/ruff/ty_repro_hf.py
  /Users/crmarsh/workspace/ruff/ty_repro_hf.py:13:9 - error: No parameter named "function" (reportCallIssue)
  /Users/crmarsh/workspace/ruff/ty_repro_hf.py:13:50 - error: No parameter named "name" (reportCallIssue)
2 errors, 0 warnings, 0 informations
```

AFAICT they have a custom dataclass transform that isn't marked with `@dataclass_transform`: https://github.com/huggingface/huggingface_hub/blob/b3a8754d6fd16ee888cc155b359464b4451afac7/src/huggingface_hub/inference/_generated/types/base.py#L32

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-21 14:02_

---

_Comment by @charliermarsh on 2025-12-21 14:12_

Fixed upstream in https://github.com/huggingface/huggingface_hub/pull/3639.

---

_Comment by @dsfaccini on 2025-12-21 14:29_

awesome, appreciated!

---

_Comment by @Wauplin on 2026-01-06 11:13_

Hey there! Maintainer of the `huggingface_hub` library here 👋. I've just released `huggingface_hub` v1.2.4 with @charliermarsh 's fix :hugs:

https://github.com/huggingface/huggingface_hub/releases/tag/v1.2.4

And confirmed locally that the repro example now works:

<img width="566" height="40" alt="Image" src="https://github.com/user-attachments/assets/d4d76f78-7b3a-43f4-a9b3-480b343cc46d" />

---

_Comment by @AlexWaygood on 2026-01-06 11:29_

Thanks so much @Wauplin!!

---

_Closed by @AlexWaygood on 2026-01-06 11:29_

---
