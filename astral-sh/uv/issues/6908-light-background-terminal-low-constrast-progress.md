```yaml
number: 6908
title: "light background terminal: low constrast progress bars"
type: issue
state: closed
author: bluss
labels:
  - help wanted
  - cli
assignees: []
created_at: 2024-09-01T09:57:09Z
updated_at: 2025-10-30T14:16:30Z
url: https://github.com/astral-sh/uv/issues/6908
synced_at: 2026-01-10T03:23:52Z
```

# light background terminal: low constrast progress bars

---

_Issue opened by @bluss on 2024-09-01 09:57_

The progress bars are low contrast on light terminal backgrounds. They don't convey progress very well.

Comparing screenshots, the progress "mark" - how far have we come - stands out much better with dark background.

![light terminal progress](https://github.com/user-attachments/assets/0e10e088-de19-40b5-876e-699ba273ce30)
![dark terminal progress](https://github.com/user-attachments/assets/8272bc17-6df5-46f3-b60c-98a0cdece8ce)

IMO pip's progress bars (I think it vendors rich) work well in comparison. One thing they have in favour of them is that they are a solid line in the horizontal direction(?), and they probably use non-ascii.

![pip on light terminal background](https://github.com/user-attachments/assets/f979f56d-9869-4d8b-a29e-30c99624d3dd)


---

_Label `cli` added by @charliermarsh on 2024-09-01 15:58_

---

_Comment by @cbrnr on 2025-10-29 09:53_

TBH, I think they are also very hard to read with a dark background:

<img width="1100" height="731" alt="Image" src="https://github.com/user-attachments/assets/4cefbba0-bbda-4bb9-9c92-8235353cf476" />

Maybe make the right portions light gray instead of green?

---

_Label `help wanted` added by @zanieb on 2025-10-29 14:51_

---

_Comment by @zanieb on 2025-10-29 14:51_

I don't know if I feel very strongly about this. I'm happy to review a change as long as it's clearly an improvement.

---

_Closed by @zanieb on 2025-10-30 14:16_

---
