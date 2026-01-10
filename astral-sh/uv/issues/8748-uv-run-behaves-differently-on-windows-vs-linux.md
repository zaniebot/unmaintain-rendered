---
number: 8748
title: uv run behaves differently on windows vs linux
type: issue
state: closed
author: linde12
labels:
  - question
assignees: []
created_at: 2024-11-01T11:10:36Z
updated_at: 2024-11-01T18:39:23Z
url: https://github.com/astral-sh/uv/issues/8748
synced_at: 2026-01-10T01:24:32Z
---

# uv run behaves differently on windows vs linux

---

_Issue opened by @linde12 on 2024-11-01 11:10_

I'm trying to run `func start` (Azure Functions Emulator) with uv, and in Linux this works fine: `uv run func start` runs `func start` with the .venv as expected. However, on Windows it is unable to run `func` as it can't spawn the process.

As a minimal example i did the same with echo:
Linux (fish shell):
![image](https://github.com/user-attachments/assets/977dd098-9770-40b9-aefc-57452decdd91)

Windows (powershell):
![image](https://github.com/user-attachments/assets/92b5516f-268d-48e1-ba8d-28cedf6df36f)

So there seems to be some mismatch between the two, but maybe there is better/preferred way of running an arbitrary program while also running/installing the uv venv?

---

_Comment by @FishAlchemist on 2024-11-01 11:25_

In Ubuntu 20, ``echo`` is a program, and I assume it's the same in other Linux distributions. It's normal for it to work in Linux. As for Windows, I believe echo doesn't exist as a standalone program. The echo in PowerShell is an alias, and in cmd, it's likely a built-in command.
![image](https://github.com/user-attachments/assets/699f74a5-3f65-41bc-99be-f96b79cfa7b0)


---

_Comment by @linde12 on 2024-11-01 11:27_

> In Ubuntu 20, echo is a program, and I assume it's the same in other Linux distributions. It's normal for it to work in Linux. As for Windows, I believe echo doesn't exist as a standalone program. The echo in PowerShell is an alias, and in cmd, it's likely a built-in command. ![image](https://private-user-images.githubusercontent.com/48265002/382266859-699f74a5-3f65-41bc-99be-f96b79cfa7b0.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzA0NjA2MjMsIm5iZiI6MTczMDQ2MDMyMywicGF0aCI6Ii80ODI2NTAwMi8zODIyNjY4NTktNjk5Zjc0YTUtM2Y2NS00MWJjLTk5YmUtZjk2Yjc5Y2ZhN2IwLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDExMDElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMTAxVDExMjUyM1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWM4NTExMjFmNzNiMjNhYmUyYzI4MzBjNGYyNzhjYmQ1MGZkMzhjZWQ4MTdlZjdhZDJkOGI2MjU2ZDYwY2Q0NzEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.Bm2CJCl6Okl5ZIEiMyNiEf65P2WfPHXmvah0W95yZCs)

@FishAlchemist Aha! This make sense, its just weird that the same happens with `func start` ..! I was unable to reproduce it on another Windows machine though, so i will investigate if there is something wrong with the first one.

---

_Comment by @FishAlchemist on 2024-11-01 11:31_

@linde12 For Windows, check the ``.venv\Scripts`` folder. If any package has an executable file, it's usually placed in this Scripts folder.

---

_Comment by @linde12 on 2024-11-01 11:35_

> @linde12 For Windows, check the `.venv\Scripts` folder. If any package has an executable file, it's usually placed in this Scripts folder.

Ah yes, but `func` is a third party tool not installed in the .venv. I just want to run this executable with the venv activated.

---

_Comment by @FishAlchemist on 2024-11-01 11:51_

> > @linde12 For Windows, check the `.venv\Scripts` folder. If any package has an executable file, it's usually placed in this Scripts folder.
> 
> Ah yes, but `func` is a third party tool not installed in the .venv. I just want to run this executable with the venv activated.

 I think your Windows environment doesn't have an executable file named ``echo`` that can be found by UV.
 However, it seems like ``echo`` isn't the program you actually want to run. Is there a possibility that you forgot to install the related program on Windows?
 
 
 UV is not limited to executing files within virtual environments only.
![image](https://github.com/user-attachments/assets/ad52f147-6eb5-4a11-9e4b-b5d3d76d31e2)




---

_Label `question` added by @charliermarsh on 2024-11-01 13:24_

---

_Comment by @linde12 on 2024-11-01 18:39_

Hi! Reporting back! `func` was installed and worked when running without uv, but we reinstalled it on my colleagues machine and then everything worked... Not complaining :smile: Closing this as it seems to be an environment issue! Thanks @FishAlchemist 

---

_Closed by @linde12 on 2024-11-01 18:39_

---
