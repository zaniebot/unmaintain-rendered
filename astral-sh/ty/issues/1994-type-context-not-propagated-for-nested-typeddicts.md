---
number: 1994
title: Type context not propagated for nested TypedDicts and lists
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - bidirectional inference
assignees: []
created_at: 2025-12-17T09:58:18Z
updated_at: 2025-12-29T22:09:59Z
url: https://github.com/astral-sh/ty/issues/1994
synced_at: 2026-01-10T01:51:14Z
---

# Type context not propagated for nested TypedDicts and lists

---

_Issue opened by @sharkdp on 2025-12-17 09:58_

Originally reported here: https://news.ycombinator.com/item?id=46299570

ty emits an `invalid-assignment` *and* a `invalid-argument-type` diagnostic for this snippet.

I didn't have time to write a proper MRE yet, but it looks like we're not propagating the type context correctly at some point here?

```py
from anthropic.types import MessageParam

data: list[MessageParam] = [{"role": "user", "content": [{"type": "text", "text": ""}]}]
```

<details><summary>Full diagnostic output</summary>
<p>


```
error[invalid-assignment]: Object of type `list[MessageParam | dict[Unknown | str, Unknown | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `list[MessageParam]`
 --> main.py:3:7
  |
1 | from anthropic.types import MessageParam
2 |
3 | data: list[MessageParam] = [{"role": "user", "content": [{"type": "text", "text": ""}]}]
  |       ------------------   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Incompatible value of type `list[MessageParam | dict[Unknown | str, Unknown | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]`
  |       |
  |       Declared type
  |
info: rule `invalid-assignment` is enabled by default

error[invalid-argument-type]: Invalid argument to key "content" with declared type `str | Iterable[TextBlockParam | ImageBlockParam | DocumentBlockParam | ... omitted 13 union elements]` on TypedDict `MessageParam`
 --> main.py:3:29
  |
1 | from anthropic.types import MessageParam
2 |
3 | data: list[MessageParam] = [{"role": "user", "content": [{"type": "text", "text": ""}]}]
  |                             ----------------------------^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^-
  |                             |                |          |
  |                             |                |          value of type `list[Unknown | dict[Unknown | str, Unknown | str]]`
  |                             |                key has declared type `str | Iterable[TextBlockParam | ImageBlockParam | DocumentBlockParam | ... omitted 13 union elements]`
  |                             TypedDict `MessageParam`
  |
info: Item declaration
  --> /home/shark/ty-reproducer/.venv/lib/python3.13/site-packages/anthropic/types/message_param.py:24:5
   |
23 |   class MessageParam(TypedDict, total=False):
24 | /     content: Required[
25 | |         Union[
26 | |             str,
27 | |             Iterable[
28 | |                 Union[
29 | |                     TextBlockParam,
30 | |                     ImageBlockParam,
31 | |                     DocumentBlockParam,
32 | |                     SearchResultBlockParam,
33 | |                     ThinkingBlockParam,
34 | |                     RedactedThinkingBlockParam,
35 | |                     ToolUseBlockParam,
36 | |                     ToolResultBlockParam,
37 | |                     ServerToolUseBlockParam,
38 | |                     WebSearchToolResultBlockParam,
39 | |                     ContentBlock,
40 | |                 ]
41 | |             ],
42 | |         ]
43 | |     ]
   | |_____- Item declared here
44 |
45 |       role: Required[Literal["user", "assistant"]]
   |
info: rule `invalid-argument-type` is enabled by default

Found 2 diagnostics

````

</p>
</details> 


---

_Label `bug` added by @sharkdp on 2025-12-17 09:58_

---

_Label `bidirectional inference` added by @sharkdp on 2025-12-17 09:58_

---

_Comment by @ibraheemdev on 2025-12-18 02:24_

At first glance, this looks like a combination of https://github.com/astral-sh/ty/issues/1576 as well as the union annotation not being handled well -- we'd likely need multi-inference here, or typed dict tagged unions.

---

_Comment by @cscanlin on 2025-12-18 02:25_

Maybe related:
```py
from typing import NotRequired, TypedDict


class Args(TypedDict):
    a: str
    b: bool
    c: NotRequired[str]


args: Args = {'a': 'hello', 'b': True}
args2: Args = {**args, 'c': 'world'}
```

```
error[missing-typed-dict-key]: Missing required key 'a' in TypedDict `Args` constructor
  --> test.py:11:15
   |
10 | args: Args = {'a': 'hello', 'b': True}
11 | args2: Args = {**args, 'c': 'world'}
   |               ^^^^^^^^^^^^^^^^^^^^^^
   |
info: rule `missing-typed-dict-key` is enabled by default

error[missing-typed-dict-key]: Missing required key 'b' in TypedDict `Args` constructor
  --> test.py:11:15
   |
10 | args: Args = {'a': 'hello', 'b': True}
11 | args2: Args = {**args, 'c': 'world'}
   |               ^^^^^^^^^^^^^^^^^^^^^^
   |
info: rule `missing-typed-dict-key` is enabled by default

Found 2 diagnostics
```

---

_Comment by @ibraheemdev on 2025-12-18 02:28_

@cscanlin that looks related to https://github.com/astral-sh/ty/issues/1332. We don't correctly handle kwarg splats in dictionary literals, and likely need special-casing for typed dicts as well.

---

_Added to milestone `Stable` by @carljm on 2025-12-22 23:33_

---

_Comment by @ibraheemdev on 2025-12-29 22:09_

After https://github.com/astral-sh/ruff/pull/21930 lands, the remaining issue here is https://github.com/astral-sh/ty/issues/2265.

---

_Closed by @ibraheemdev on 2025-12-29 22:09_

---
