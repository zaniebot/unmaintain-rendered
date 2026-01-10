```yaml
number: 892
title: "Can't infer type from pydantic field"
type: issue
state: closed
author: scosman
labels: []
assignees: []
created_at: 2025-07-25T15:40:37Z
updated_at: 2025-07-25T16:40:33Z
url: https://github.com/astral-sh/ty/issues/892
synced_at: 2026-01-10T02:06:24Z
```

# Can't infer type from pydantic field

---

_Issue opened by @scosman on 2025-07-25 15:40_

### Summary

Ty is not inferring the correct type for a pydantic field. It's getting Unknown for a typed field, and requires manual casting. 
<img width="825" height="83" alt="Image" src="https://github.com/user-attachments/assets/98fa954d-3023-41c3-b3cf-10cc4c17c1a8" />

Notice the IDE knows the type by ty infers "Unknown". Pyright also gets the type correct. casting can force the type, but shouldn't be necessary.

Here's how the pydantic class is setup.
```
class EvalOutputScore(BaseModel):
    type: TaskOutputRatingType = Field(
        description="The type of rating to use ('five_star', 'pass_fail', 'pass_fail_critical')."
    )
```

### Version

_No response_

---

_Comment by @scosman on 2025-07-25 16:13_

Note: I'm having a very hard time getting a minimal repo of this. I'm working on it and will try to post one here.

---

_Comment by @scosman on 2025-07-25 16:40_

Closing: I found the issue. I found the source higher up the tree.

---

_Closed by @scosman on 2025-07-25 16:40_

---
