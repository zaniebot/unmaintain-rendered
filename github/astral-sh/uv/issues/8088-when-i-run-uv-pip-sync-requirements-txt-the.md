---
number: 8088
title: "when  I run  `uv  pip  sync requirements.txt`   , the  environment  is  not be  updated  "
type: issue
state: closed
author: Super1Windcloud
labels:
  - question
assignees: []
created_at: 2024-10-10T12:36:47Z
updated_at: 2024-10-12T14:59:33Z
url: https://github.com/astral-sh/uv/issues/8088
synced_at: 2026-01-07T13:12:17-06:00
---

# when  I run  `uv  pip  sync requirements.txt`   , the  environment  is  not be  updated  

---

_Issue opened by @Super1Windcloud on 2024-10-10 12:36_

win11

![image](https://github.com/user-attachments/assets/6c962536-4e1f-4647-8e6a-e498eb254a3d)

#  but   pyproject.toml    
![image](https://github.com/user-attachments/assets/bc5c6734-a0e8-4977-bdf9-d8ca026255ba)


---

_Comment by @Super1Windcloud on 2024-10-10 13:09_

So,  I  think this command   of  ` uv add -r requirements.txt   ` can  instead it  .  

---

_Comment by @charliermarsh on 2024-10-12 14:59_

Yeah I think you're looking for `uv add` here as you mentioned above.

---

_Closed by @charliermarsh on 2024-10-12 14:59_

---

_Label `question` added by @charliermarsh on 2024-10-12 14:59_

---
