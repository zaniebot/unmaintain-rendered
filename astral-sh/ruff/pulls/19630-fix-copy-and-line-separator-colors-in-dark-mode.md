```yaml
number: 19630
title: Fix copy and line separator colors in dark mode
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: micha/fix-dark-mode-colors
created_at: 2025-07-30T07:21:37Z
updated_at: 2025-07-30T16:18:39Z
url: https://github.com/astral-sh/ruff/pull/19630
synced_at: 2026-01-12T15:56:43Z
```

# Fix copy and line separator colors in dark mode

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/19613

Note: We have the same issue in the ty and uv documentation.

## Test Plan

Before 
<img width="1210" height="677" alt="Screenshot 2025-07-30 at 09 22 39" src="https://github.com/user-attachments/assets/4a5864e9-6194-450a-b4ac-e45a205295c9" />

After


<img width="1671" height="923" alt="Screenshot 2025-07-30 at 09 36 05" src="https://github.com/user-attachments/assets/0eb8c43a-4075-4c72-81d9-3fdee1bfedc0" />


I also decided to change the color for light mode too because the visibility wasn't great either:

Before

<img width="1671" height="923" alt="Screenshot 2025-07-30 at 09 37 36" src="https://github.com/user-attachments/assets/a40f4754-c049-4c8b-b6fe-a56a0ae6fe71" />

After
<img width="1089" height="576" alt="Screenshot 2025-07-30 at 09 37 09" src="https://github.com/user-attachments/assets/a986a85b-9012-46d4-bdcb-7eb01fb7a005" />



---

_Label `documentation` added by @MichaReiser on 2025-07-30 07:21_

---

_Review requested from @charliermarsh by @MichaReiser on 2025-07-30 07:38_

---

_Review requested from @zanieb by @MichaReiser on 2025-07-30 07:38_

---

_@sharkdp approved on 2025-07-30 07:44_

---

_Comment by @zanieb on 2025-07-30 13:32_

Thanks! Would you mind posting a PR elsewhere too?

I guess someday we should switch to a shared repository for these utilities somehow

---

_Comment by @MichaReiser on 2025-07-30 14:08_

> Thanks! Would you mind posting a PR elsewhere too?

That's what I had in mind. I just wanted to wait to get approval on my arbitrary color choice :)

---

_Merged by @MichaReiser on 2025-07-30 14:08_

---

_Closed by @MichaReiser on 2025-07-30 14:08_

---

_Branch deleted on 2025-07-30 14:08_

---

_Comment by @zanieb on 2025-07-30 16:18_

I was too thrown off by the changes to the capture window in the screenshots to pass judgement on that.

---
