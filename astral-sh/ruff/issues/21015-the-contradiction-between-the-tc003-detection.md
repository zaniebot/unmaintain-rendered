```yaml
number: 21015
title: The contradiction between the TC003 detection rule and the pydantic model
type: issue
state: closed
author: LIghtJUNction
labels:
  - question
assignees: []
created_at: 2025-10-21T11:45:05Z
updated_at: 2025-10-21T11:52:17Z
url: https://github.com/astral-sh/ruff/issues/21015
synced_at: 2026-01-12T15:54:57Z
```

# The contradiction between the TC003 detection rule and the pydantic model

---

_@LIghtJUNction_

### Question

<img width="995" height="153" alt="Image" src="https://github.com/user-attachments/assets/01e2ad47-056e-4b49-b81d-6201bfdfcaf9" />
As shown in the image, I should modify it like this:

<img width="675" height="131" alt="Image" src="https://github.com/user-attachments/assets/50e4f870-08bf-4e5c-b749-2eff583d2d8f" />

But if this is the case (the pydantic BaseModel field is annotated with that type):

<img width="329" height="314" alt="Image" src="https://github.com/user-attachments/assets/e3e88765-bbbf-4e8f-bbf5-7564970bf902" />
The results are as follows:

<img width="1213" height="175" alt="Image" src="https://github.com/user-attachments/assets/ce3aa0cb-07b2-4744-a7d3-72a9a15122a2" />

pydantic relies on type annotations at runtime, how can this be perfectly solved?

### Version

_No response_

---

_Label `question` added by @LIghtJUNction on 2025-10-21 11:45_

---

_Closed by @LIghtJUNction on 2025-10-21 11:52_

---
