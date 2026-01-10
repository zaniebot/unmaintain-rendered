```yaml
number: 9667
title: uv python list report error
type: issue
state: closed
author: cjdxhjj
labels:
  - windows
  - needs-mre
assignees: []
created_at: 2024-12-06T01:51:34Z
updated_at: 2024-12-12T11:07:59Z
url: https://github.com/astral-sh/uv/issues/9667
synced_at: 2026-01-10T04:36:21Z
```

# uv python list report error

---

_Issue opened by @cjdxhjj on 2024-12-06 01:51_

when i run the uv python list
![Image](https://github.com/user-attachments/assets/71b49a78-02ae-4f88-85bd-2f98a4c01cbb)
i have install the python with windows store, the location at C:\Program Files\WindowsApps\PythonSoftwareFoundation.Python.3.12_3.12.2288.0_x64__qbz5n2kfra8p0



---

_Comment by @cjdxhjj on 2024-12-06 01:52_

![Image](https://github.com/user-attachments/assets/c5534248-8321-483f-abae-335040211884)


---

_Comment by @charliermarsh on 2024-12-06 01:54_

Do you know what the error says?

---

_Label `needs-mre` added by @charliermarsh on 2024-12-06 02:13_

---

_Comment by @cjdxhjj on 2024-12-06 08:17_

The path shown in the error message is the path where I installed the software last time.

---

_Comment by @cjdxhjj on 2024-12-06 08:19_

the windows store install the python at appdata before, but now it at program files, i install the python first then install the uv
but the uv python list not working, is the uv read some configuration that broken ?

---

_Comment by @cjdxhjj on 2024-12-06 08:21_

![Image](https://github.com/user-attachments/assets/df9d8de0-8e45-4fb5-a4dc-23e39a732517)


---

_Comment by @cjdxhjj on 2024-12-06 08:28_

![Image](https://github.com/user-attachments/assets/875fb966-0cec-4f73-a0f5-52a842f952b2)


---

_Comment by @cjdxhjj on 2024-12-06 08:29_

![Image](https://github.com/user-attachments/assets/2d083112-0230-4cf0-9bb7-1dc3a5c368bd)
![Image](https://github.com/user-attachments/assets/66ee152a-b3af-4bff-9ab4-b9b117b2e117)


---

_Comment by @cjdxhjj on 2024-12-06 08:33_

i have searched my register and found nothing about the error path

---

_Comment by @charliermarsh on 2024-12-06 11:48_

Thanks. I _think_ we should be "skipping" that path but I need to double check. 

---

_Label `windows` added by @charliermarsh on 2024-12-06 11:48_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-06 12:33_

---

_Comment by @cjdxhjj on 2024-12-10 02:09_

![Image](https://github.com/user-attachments/assets/89e963b0-9d6c-4f7f-9d8b-d6ee0005fdea)
when i upgrade to 0.5.7, it report that error

---

_Closed by @charliermarsh on 2024-12-10 02:59_

---

_Closed by @charliermarsh on 2024-12-10 02:59_

---

_Comment by @cjdxhjj on 2024-12-12 11:07_

![Image](https://github.com/user-attachments/assets/a05fd741-f912-4885-a0c1-10b160fcd6f9)
@charliermarsh 


---

_Comment by @cjdxhjj on 2024-12-12 11:07_

the issure not resolved

---
