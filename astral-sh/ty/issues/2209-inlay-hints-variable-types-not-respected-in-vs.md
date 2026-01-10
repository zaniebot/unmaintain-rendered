---
number: 2209
title: Inlay Hints Variable Types Not Respected in VS Code
type: issue
state: closed
author: agedburd
labels:
  - question
  - needs-info
assignees: []
created_at: 2025-12-24T20:32:41Z
updated_at: 2025-12-29T16:54:03Z
url: https://github.com/astral-sh/ty/issues/2209
synced_at: 2026-01-10T01:51:14Z
---

# Inlay Hints Variable Types Not Respected in VS Code

---

_Issue opened by @agedburd on 2025-12-24 20:32_

### Summary

The "Inlay Hints: Variable Types" setting for both User and Workspace are not respected in the VS Code extension.

<img width="588" height="345" alt="Image" src="https://github.com/user-attachments/assets/43c0f4ac-8ae3-43ed-b787-4389cbd6fa65" />

<img width="583" height="326" alt="Image" src="https://github.com/user-attachments/assets/9d503ae8-dc75-4489-a4ef-5a8f34e0ef69" />

VS Code version 1.107.1

Example code:
``` python
import pandas as pd
temp = {}
ref_df = pd.DataFrame.from_records(
    list(temp.values()),
    index=list(temp.keys()),
    columns=["start_date", "end_date"],
)
```

### Version

ty 0.0.6

---

_Comment by @ntBre on 2025-12-24 21:17_

This setting seems to be working when I tried to reproduce it locally. Could these inlay hints be coming from pylance?

![Image](https://github.com/user-attachments/assets/8b5d3e71-0291-4ad1-abb4-67cf9afc122b)

---

_Label `question` added by @ntBre on 2025-12-24 21:17_

---

_Label `needs-info` added by @dhruvmanila on 2025-12-25 09:19_

---

_Comment by @agedburd on 2025-12-29 16:32_

I do have pylance installed, but have my language server set to None as per the editor integration page:

<img width="349" height="133" alt="Image" src="https://github.com/user-attachments/assets/22513082-74a6-4681-a75f-8f11030547c9" />

The default for pylance, and my current setting, is to have variable type inlay hints off:

<img width="883" height="452" alt="Image" src="https://github.com/user-attachments/assets/a97d0ceb-d771-4105-a3bd-dcfd5a40c48c" />

Updated to 0.0.8 and this is still occurring. It's definitely coming from ty or some unexpected interaction with ty.

https://github.com/user-attachments/assets/e466b5eb-a0fb-4d6d-bdf1-17b65693adec

https://github.com/user-attachments/assets/fcd04906-e04c-43a1-8254-f5e809914d7e

---

_Comment by @agedburd on 2025-12-29 16:50_

I just realized I also had a ty dependency in my pyproject.toml that was causing the whole thing. Thank you for your time.

---

_Closed by @agedburd on 2025-12-29 16:50_

---

_Comment by @MichaReiser on 2025-12-29 16:54_

No worries. I'm glad you figured it out and thanks for letting us know that it's now working. Happy holidays

---
