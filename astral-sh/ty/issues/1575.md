```yaml
number: 1575
title: Go-to definition in hover
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-11-17T10:56:13Z
updated_at: 2025-11-20T20:59:46Z
url: https://github.com/astral-sh/ty/issues/1575
synced_at: 2026-01-10T01:58:59Z
```

# Go-to definition in hover

---

_Issue opened by @MichaReiser on 2025-11-17 10:56_

In rust-analyzer, I can click on `Command` to jump to its definition. 

<img width="595" height="315" alt="Image" src="https://github.com/user-attachments/assets/28156db4-d882-4f3e-8458-1d08a20e4946" />

We should also support this in ty

```py
from typing import TypedDict

class Test(TypedDict):
    name: str
    age: int



tests = [Test(), Test()]


for test in tests:
    pass
```

Hovering `test` reveals `Test`, it should be possible to click on `Test` and navigate to its definition



---

_Label `server` added by @MichaReiser on 2025-11-17 10:56_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-17 10:56_

---

_Comment by @dhruvmanila on 2025-11-17 12:10_

> In rust-analyzer, I can click on `Command` to jump to its definition.

But the code snippet that you've shared via a screenshot looks Python :)

---

_Comment by @MichaReiser on 2025-11-17 12:18_

> > In rust-analyzer, I can click on `Command` to jump to its definition.
> 
> But the code snippet that you've shared via a screenshot looks Python :)

Yes. I was using ty while working on ty_benchmark and was very annoyed wheb I couldn't click on Command

---

_Comment by @dhruvmanila on 2025-11-17 12:46_

Oh, sorry for being unclear, I thought you meant to provide a Rust code snippet demonstrating what it looks like in rust-analyzer. Is this what you're referring to in rust-analyzer? Specifically, the "Goto" links at the bottom of the hover window.

<img width="1017" height="467" alt="Image" src="https://github.com/user-attachments/assets/012406c6-33e9-40b3-9d9f-1d0f50b162aa" />

---

_Comment by @Gankra on 2025-11-20 20:57_

Note the entire "Go to..." pane is 100% a non-standard extension of rust-analyzer. Which is to say, we would have to implement UI for it in the vscode extension and it would only work in the vscode extension. I agree it rules, I was sad when I discovered this last week.

https://github.com/rust-lang/rust-analyzer/blob/2efc80078029894eec0699f62ec8d5c1a56af763/docs/book/src/contributing/lsp-extensions.md#hover-actions

---

_Comment by @MichaReiser on 2025-11-20 20:59_

That's indeed very disappointing

---
