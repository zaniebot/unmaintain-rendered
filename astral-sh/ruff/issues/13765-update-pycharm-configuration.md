```yaml
number: 13765
title: Update PyCharm Configuration
type: issue
state: closed
author: IdoPort
labels:
  - documentation
assignees: []
created_at: 2024-10-15T20:44:56Z
updated_at: 2025-05-20T15:44:03Z
url: https://github.com/astral-sh/ruff/issues/13765
synced_at: 2026-01-12T15:54:53Z
```

# Update PyCharm Configuration

---

_@IdoPort_

Since not specifying the check is deprecated we should change the picture to this one:
![PyCharmRuffConfiguration](https://github.com/user-attachments/assets/1eb617e1-5ec4-45cc-ac1c-10d80033213f)


---

_Comment by @dhruvmanila on 2024-10-16 04:32_

Thanks for noticing. Yeah, we should update the screenshot.

---

_Label `documentation` added by @dhruvmanila on 2024-10-16 04:32_

---

_Closed by @dhruvmanila on 2024-10-16 04:41_

---

_Comment by @Lexachoc on 2025-05-09 16:34_

I installed `ruff` with `pip install ruff`

And in PyCharm 2025.1 (Build #PY-251.23774.444, built on April 15, 2025), I have to use:
`$PyInterpreterDirectory$/Scripts/ruff`
instead of 
`$PyInterpreterDirectory$/ruff`

Maybe the image should be updated?

![Image](https://github.com/user-attachments/assets/933b9f0d-f5c9-4bbb-8b34-efb75a6f9042)

and maybe providing the texts would be even better:


Name: 
```
ruff
```

Description: 
```
Run ruff on project sources
```

Program: 
```
$PyInterpreterDirectory$/Scripts/ruff
```

Arguements: 
```
check $ContentRoot$
```

Working directory: 
```
$ContentRoot$
```



---

_Comment by @dhruvmanila on 2025-05-20 15:44_

Happy to accept a PR that replaces the screenshot with a text equivalent, not sure what the format should look like but we can discuss / play around it in the PR.

---
