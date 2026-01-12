```yaml
number: 1515
title: Add a background color to the top image in the README 
type: issue
state: closed
author: obi1kenobi
labels:
  - documentation
assignees: []
created_at: 2022-12-31T17:20:51Z
updated_at: 2022-12-31T22:47:33Z
url: https://github.com/astral-sh/ruff/issues/1515
synced_at: 2026-01-12T15:54:41Z
```

# Add a background color to the top image in the README 

---

_@obi1kenobi_

The top image in the README is unreadable on dark mode GitHub: its background is transparent, and it gets rendered as black-on-black.
![image](https://user-images.githubusercontent.com/2348618/210151069-a715d8b9-7d63-422c-b521-14e87115507b.png)

I'm not sure it's possible to change its color choices based on the light mode/dark mode setting. Adding an off-white or light grey background color would also solve the problem.

---

_Comment by @charliermarsh on 2022-12-31 17:28_

Good call, thank you :)

---

_Label `documentation` added by @charliermarsh on 2022-12-31 17:29_

---

_Comment by @Jackenmen on 2022-12-31 17:35_

> I'm not sure it's possible to change its color choices based on the light mode/dark mode setting

You can, see: https://github.blog/changelog/2022-08-15-specify-theme-context-for-images-in-markdown-ga/

Although, this can probably also be resolved by adding a white stroke to the text so that it's visible both on dark and bright background.

---

_Comment by @charliermarsh on 2022-12-31 17:41_

I’ll look into it. It’s rendered via Vega but I have to dig up the source.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-31 18:06_

---

_Comment by @charliermarsh on 2022-12-31 22:47_

Actually looks... very cool:

<img width="1042" alt="Screen Shot 2022-12-31 at 5 47 02 PM" src="https://user-images.githubusercontent.com/1309177/210156953-2c41b4c1-a8a9-49d6-983e-8b9420f66040.png">


---

_Closed by @charliermarsh on 2022-12-31 22:47_

---
