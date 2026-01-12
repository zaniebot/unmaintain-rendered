```yaml
number: 1808
title: "`unused-ignore-comment` diagnostics are very verbose"
type: issue
state: open
author: AlexWaygood
labels:
  - diagnostics
assignees: []
created_at: 2025-12-08T15:30:39Z
updated_at: 2025-12-08T17:27:36Z
url: https://github.com/astral-sh/ty/issues/1808
synced_at: 2026-01-12T15:54:25Z
```

# `unused-ignore-comment` diagnostics are very verbose

---

_@AlexWaygood_

Two `unused-ignore-comment` diagnostics are enough to take up most of my monitor with my terminal full screen. It feels like the rendering of the autofix doesn't add much on top of what the primary annotation shows:

<details>
<summary>Screenshot</summary>

<img width="1782" height="1716" alt="Image" src="https://github.com/user-attachments/assets/85eb9eba-7b21-495e-a1e9-b62b7a9f3676" />

</details>

But if anything, I think I'd actually prefer to keep the autofix rendering and get rid of the primary annotation? It's definitely useful information to know that there is an autofix, and I love that the diff rendering shows me exactly how to fix the diagnostic even I don't want to opt into autofixes.

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-08 15:30_

---

_Comment by @MichaReiser on 2025-12-08 16:10_

I'm not sure I fully agree. I do think it's useful to highlight the unused comment, especially if the code spawns multiple lines. 

This is also a more general discussion on how we want to render fixes/primary annotations.

---

_Comment by @AlexWaygood on 2025-12-08 17:27_

It might just be that our context windows are too big for the fix rendering? I don't find this diagnostic from rustc to be nearly as verbose, and it's basically doing the same thing as our diagnostics but with much smaller context windows:

<img width="2158" height="534" alt="Image" src="https://github.com/user-attachments/assets/4ee58a6b-4030-43ce-8039-1a1dc051beaa" />

---
