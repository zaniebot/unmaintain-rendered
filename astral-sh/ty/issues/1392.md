```yaml
number: 1392
title: Missing completions if cursor is at the end of the file
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - server
  - completions
assignees: []
created_at: 2025-10-17T19:46:22Z
updated_at: 2025-10-21T09:24:33Z
url: https://github.com/astral-sh/ty/issues/1392
synced_at: 2026-01-10T02:06:25Z
```

# Missing completions if cursor is at the end of the file

---

_Issue opened by @sharkdp on 2025-10-17 19:46_

I recently added these tests: https://github.com/astral-sh/ruff/blob/8ca2b5555d47e211c04136637477c5227172d885/crates/ty_ide/src/completion.rs#L3899-L3946

They show that
```py
def f[T: str](msg: T):
    msg.<CURSOR>
```

should show completions like `upper` and `capitalize`. This does work in the LSP with neovim. But it does not work with VS Code or in the playground:

neovim:
<img width="429" height="238" alt="Image" src="https://github.com/user-attachments/assets/5f9f8e44-4d93-40ab-bb0a-e2bebc1971db" />

VS Code:
<img width="588" height="170" alt="Image" src="https://github.com/user-attachments/assets/55108d96-4c8b-4cbc-a089-f7436d9f8e00" />

Playground:
<img width="635" height="163" alt="Image" src="https://github.com/user-attachments/assets/f1dcc290-809f-4683-813d-a91aafaf7aa3" />

What's very weird is that I *can* see those completions if I make other random changes to the file. For example, if I add a comment in that function:

<img width="459" height="213" alt="Image" src="https://github.com/user-attachments/assets/2d5db72c-6dab-4d09-a88c-935c5a1675e0" />


---

_Label `bug` added by @sharkdp on 2025-10-17 19:46_

---

_Label `server` added by @sharkdp on 2025-10-17 19:46_

---

_Label `completions` added by @sharkdp on 2025-10-17 19:46_

---

_Renamed from "Completions different between tests, neovim, VS Code, and playground" to "Missing completions in playground and VS Code (caching problem?)" by @sharkdp on 2025-10-17 19:49_

---

_Comment by @sharkdp on 2025-10-17 19:49_

This is not related to the `TypeVar` example:

<img width="621" height="172" alt="Image" src="https://github.com/user-attachments/assets/969f8ad3-23e4-4b50-a6d3-2f8513c17f41" />

---

_Comment by @sharkdp on 2025-10-17 19:49_

Will move this to the beta milestone as this looks quite bad.

---

_Added to milestone `Beta` by @sharkdp on 2025-10-17 19:50_

---

_Renamed from "Missing completions in playground and VS Code (caching problem?)" to "Missing completions in playground and VS Code" by @sharkdp on 2025-10-17 19:59_

---

_Comment by @sharkdp on 2025-10-17 20:00_

A single space or newline after the attribute expression is enough to make this work. That's probably why it works in the tests. And maybe neovim just always has a trailing newline in its buffer, compared to VS Code and the Playground.

---

_Assigned to @sharkdp by @sharkdp on 2025-10-17 20:00_

---

_Renamed from "Missing completions in playground and VS Code" to "Missing completions if cursor is at the end of the file" by @sharkdp on 2025-10-17 20:10_

---

_Unassigned @sharkdp by @sharkdp on 2025-10-20 08:58_

---

_Assigned to @MichaReiser by @sharkdp on 2025-10-20 08:58_

---

_Comment by @MichaReiser on 2025-10-20 09:45_

This seems related/the same https://github.com/astral-sh/ty/issues/1278

---

_Comment by @sharkdp on 2025-10-20 09:48_

Oh, yes. Completely forgot about that (closed now). At the time, I thought it was only playground specific (probably because I only compared it to neovim behavior), and thought that maybe something just wasn't exposed to the playground.

---

_Comment by @MichaReiser on 2025-10-20 09:50_

I suspect that is due to our heuristic for "this is the current scope" is a bit naive https://github.com/astral-sh/ruff/pull/18281#discussion_r2105173847

More specifically, the issue is that the function's range ends right after the `.`. 

Funnily, the completions work if you add a trailing comment (which doesn't change the function's range, so maybe that's wrong?))

```py
def f[T: str](msg: T):
    msg. # comment
```



---

_Comment by @sharkdp on 2025-10-20 09:53_

> A single space or newline after the attribute expression is enough to make this work.

Yes

---

_Comment by @MichaReiser on 2025-10-20 09:59_

Ah, these are the tests that I thought this issue is related to, but maybe not? I'll keep digging

https://github.com/astral-sh/ruff/blob/6e7ff0706564cb938c154b5a5f9993c1118085e0/crates/ty_ide/src/completion.rs#L1355-L1454

---

_Comment by @sharkdp on 2025-10-20 10:04_

Well as far as I understood, the problem was that if the cursor is at the end of the file, there is an additional Newline token "at the offset of the cursor". This is why https://github.com/astral-sh/ruff/pull/20951 simply attempts to strip that token off, and then everything works as expected. But that seems like a bit of a hack. That newline probably shouldn't be there in the first place? Or at least, we should identify that the cursor is behind the dot, not behind the newline.

---

_Comment by @sharkdp on 2025-10-20 10:05_

<img width="783" height="492" alt="Image" src="https://github.com/user-attachments/assets/649facc2-1f5c-4f95-b780-17a473b41b08" />

---

_Comment by @MichaReiser on 2025-10-20 11:41_

Interestingly, this works with cursor tests.

> That newline probably shouldn't be there in the first place? Or at least, we should identify that the cursor is behind the dot, not behind the newline.

The newline token is correct. Every statement must be terminated by a newline

---

_Comment by @MichaReiser on 2025-10-20 11:50_

Okay, I think I know what the issue is. It's that `Newline` has an empty range. And that is only the case for the newline at the very end of the file. All other newlines (ignoring newlines inserted by error recovery) are guaranteed to have a non empty range


---

_Closed by @MichaReiser on 2025-10-21 09:24_

---
