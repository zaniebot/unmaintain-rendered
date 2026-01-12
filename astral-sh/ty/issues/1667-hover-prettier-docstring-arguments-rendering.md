```yaml
number: 1667
title: "Hover: Prettier docstring arguments rendering"
type: issue
state: open
author: hermeschen1116
labels:
  - server
assignees: []
created_at: 2025-11-28T14:32:41Z
updated_at: 2025-11-28T15:08:32Z
url: https://github.com/astral-sh/ty/issues/1667
synced_at: 2026-01-12T15:54:25Z
```

# Hover: Prettier docstring arguments rendering

---

_@hermeschen1116_

Currently, docstring in hover information is just plain text. I think it's better to have syntax highlight for readability. 

---

_Renamed from "Colorize docstring in hover information" to "Syntax highlighting for docstrings" by @MichaReiser on 2025-11-28 14:34_

---

_Comment by @MichaReiser on 2025-11-28 14:36_

Can you share an example where syntax highlighting isn't working in docstrings and tell us what editor you're using?

<img width="1460" height="800" alt="Image" src="https://github.com/user-attachments/assets/8ae2a3ef-79cf-4511-9e91-5ceab5b34b2e" />



---

_Label `needs-info` added by @MichaReiser on 2025-11-28 14:36_

---

_Comment by @hermeschen1116 on 2025-11-28 14:40_

I'm using zed, and the docstring in hover information is like this in my editor.
<img width="793" height="485" alt="Image" src="https://github.com/user-attachments/assets/b50896a8-1a63-48b1-bc0b-ef8409be9784" />


---

_Comment by @MichaReiser on 2025-11-28 14:42_

Can you share the raw dostring?

I don't think we can implement syntax highlighting for e.g. `service_url(HttpUrl | pathlib.Path)`. The best we can do is to wrap that part in backticks because all the rendering is done by the client and what the server (ty) returns is just markdown

---

_Comment by @Gankra on 2025-11-28 14:42_

Also what version of the ty lsp are you running? We only released the new docstring rendering a couple days ago.

---

_Comment by @hermeschen1116 on 2025-11-28 14:44_

@MichaReiser 
Below is my code.
<img width="977" height="658" alt="Image" src="https://github.com/user-attachments/assets/d2c43cc1-8c29-459d-ad6d-f3d6759561bf" />
And this one is what is showed in hover information
<img width="746" height="420" alt="Image" src="https://github.com/user-attachments/assets/834ed79b-623a-4e1f-b980-efdfec73a457" />

---

_Comment by @hermeschen1116 on 2025-11-28 14:45_

> Also what version of the ty lsp are you running? We only released the new docstring rendering a couple days ago.

I'm using the latest one, 0.0.1-alpha.28.

---

_Comment by @MichaReiser on 2025-11-28 14:52_



Thanks, that's helpful. 

I think what you're looking for here is that ty parses the docstring and applies custom formatting similar to what Pylance does

Pylance

<img width="1460" height="800" alt="Image" src="https://github.com/user-attachments/assets/51cbf08d-e15b-4d17-8e67-c1555431b11a" />

ty

<img width="1460" height="800" alt="Image" src="https://github.com/user-attachments/assets/449d95ed-e290-4e8d-bf52-aa19cb626e48" />

---

_Label `needs-info` removed by @MichaReiser on 2025-11-28 14:53_

---

_Label `server` added by @MichaReiser on 2025-11-28 14:53_

---

_Renamed from "Syntax highlighting for docstrings" to "Hover: Prettier arguments rendering" by @MichaReiser on 2025-11-28 14:53_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-28 14:54_

---

_Comment by @hermeschen1116 on 2025-11-28 15:03_

That sounds like what I was hoping for, and it’s even better with colour!

---

_Renamed from "Hover: Prettier arguments rendering" to "Hover: Prettier docstring arguments rendering" by @Gankra on 2025-11-28 15:08_

---
