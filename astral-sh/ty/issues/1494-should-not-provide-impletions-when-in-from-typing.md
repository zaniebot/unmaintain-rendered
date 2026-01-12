```yaml
number: 1494
title: "Should not provide impletions when in `from typing i<CURSOR>`."
type: issue
state: closed
author: MatthewMckee4
labels:
  - server
  - completions
assignees: []
created_at: 2025-11-06T11:55:57Z
updated_at: 2025-11-10T17:58:53Z
url: https://github.com/astral-sh/ty/issues/1494
synced_at: 2026-01-12T15:54:25Z
```

# Should not provide impletions when in `from typing i<CURSOR>`.

---

_@MatthewMckee4_

### Summary

<img width="815" height="293" alt="Image" src="https://github.com/user-attachments/assets/e25d21e7-ca6b-44ae-88d3-4279e3f31395" />

### Version

_No response_

---

_Label `server` added by @BurntSushi on 2025-11-06 11:58_

---

_Label `completions` added by @BurntSushi on 2025-11-06 11:58_

---

_Comment by @BurntSushi on 2025-11-06 12:00_

Ideally, we'd provide a completion for the `import` keyword. But providing no completions I think would be an improvement over the status quo.

---

_Comment by @MatthewMckee4 on 2025-11-06 12:12_

How do we provide specific keyword completions? Do we do this already anywhere?

---

_Comment by @BurntSushi on 2025-11-06 12:16_

This is all we have at the moment:

https://github.com/astral-sh/ruff/blob/0c7b93363e6b5b43b8f36adbd6e3adf1a13374c2/crates/ty_ide/src/completion.rs#L280-L310

But those are the keywords that are also valid Python values. For the rest of the keywords, I think you'd probably just want to fill in a `Completion` with the name and `CompletionKind::Keyword`. I think everything else would be absent.

---

_Comment by @MatthewMckee4 on 2025-11-06 12:19_

Okay I could rework my PR to provide `import` here.

If the user typed `from typing aa` for example, should we still provide `import`. That makes sense to me i think

---

_Comment by @BurntSushi on 2025-11-06 12:21_

Probably yeah.

---

_Closed by @BurntSushi on 2025-11-10 16:59_

---

_Comment by @pyscripter on 2025-11-10 17:58_

While you are at it, you should not provide completion in

import numpy as |

---
