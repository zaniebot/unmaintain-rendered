```yaml
number: 10813
title: Stable icon on the website (dark mode) is barely visible
type: issue
state: closed
author: Werni2A
labels:
  - bug
  - documentation
  - help wanted
assignees: []
created_at: 2024-04-07T13:58:17Z
updated_at: 2025-07-09T23:11:43Z
url: https://github.com/astral-sh/ruff/issues/10813
synced_at: 2026-01-12T15:54:50Z
```

# Stable icon on the website (dark mode) is barely visible

---

_@Werni2A_

In dark mode the 'stable' icon looks as if the rule were not fully implemented, because it's icon is rendered so lightly. See the screenshots for comparison. Looks like there is some kind of transparency issue.

| Light Mode | Dark Mode |
|-|-|
| ![image](https://github.com/astral-sh/ruff/assets/58232418/a683e903-8c12-4f19-bdd9-3203887b88a1) | ![image](https://github.com/astral-sh/ruff/assets/58232418/3060876e-a2ea-4f2c-9cec-5363e901be29) |


---

_Comment by @charliermarsh on 2024-04-07 16:46_

Thanks!

---

_Label `bug` added by @charliermarsh on 2024-04-07 16:46_

---

_Label `documentation` added by @charliermarsh on 2024-04-07 16:46_

---

_Label `help wanted` added by @charliermarsh on 2024-04-07 16:46_

---

_Comment by @augustelalande on 2024-04-07 17:47_

@Werni2A which system and browser are you using? It doesn't look nearly as dark for me (Windows 11, Chrome).
![image](https://github.com/astral-sh/ruff/assets/5696119/046649fc-e0d3-411d-bf07-5f92d23ba433)



---

_Comment by @Werni2A on 2024-04-07 19:36_

Hi, I'm using Firefox 124 on Ubuntu 22.04.

---

_Comment by @augustelalande on 2024-04-08 01:29_

I'm not able to reproduce on Firefox windows
![image](https://github.com/astral-sh/ruff/assets/5696119/b46651bb-e613-4c7e-8263-819a9fb7b531)


---

_Comment by @autinerd on 2024-04-08 06:59_

That's one caveat of using emojis, the rendering is different on different systems. 

---

_Comment by @MeGaGiGaGon on 2025-07-09 23:07_

This was fixed in #18297, both the checkmark icon's usage and half-transparent icons have been removed.

Comparison at E502 using [wayback machine](https://web.archive.org/web/20250130073340/https://docs.astral.sh/ruff/rules/#error-e):

|Old|New|
|----|-----|
|<img width="76" height="185" alt="Image" src="https://github.com/user-attachments/assets/c4513c88-076b-4bf6-8543-ddeba08c1bb9" />|<img width="81" height="182" alt="Image" src="https://github.com/user-attachments/assets/c326846c-0247-4140-8f60-1f4089dd5f4c" />|



---

_Closed by @charliermarsh on 2025-07-09 23:11_

---

_Comment by @charliermarsh on 2025-07-09 23:11_

Thanks!

---
