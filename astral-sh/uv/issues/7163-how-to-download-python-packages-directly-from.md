---
number: 7163
title: How to download Python packages directly from cache to avoid having to download them from the network every time
type: issue
state: closed
author: WangZhongDian
labels:
  - question
assignees: []
created_at: 2024-09-07T09:56:22Z
updated_at: 2024-09-10T02:12:24Z
url: https://github.com/astral-sh/uv/issues/7163
synced_at: 2026-01-10T01:24:11Z
---

# How to download Python packages directly from cache to avoid having to download them from the network every time

---

_Issue opened by @WangZhongDian on 2024-09-07 09:56_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @charliermarsh on 2024-09-07 11:59_

They should be downloaded from the cache by default. Can you share a command with output to help me understand what you're seeing?

---

_Label `question` added by @charliermarsh on 2024-09-07 11:59_

---

_Comment by @WangZhongDian on 2024-09-07 12:06_

@charliermarsh 
I have already downloaded the torch package and I want to test downloading the same torch package in another file, but UV still downloaded it from the network and not from the cache
![截图 2024-09-07 20-02-22](https://github.com/user-attachments/assets/147d882e-d80e-48b3-a581-8aa2b155063b)
![image](https://github.com/user-attachments/assets/402835ab-65bc-47d5-b902-00c2e9687769)



---

_Comment by @WangZhongDian on 2024-09-08 10:19_

@charliermarsh 
> 我已经下载了 torch 软件包，并且我想在另一个文件中测试下载相同的 torch 软件包，但 UV 仍然从网络而不是缓存下载了它 ![截图 2024-09-07 20-02-22](https://private-user-images.githubusercontent.com/103353084/365366717-147d882e-d80e-48b3-a581-8aa2b155063b.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjU3OTEwMzgsIm5iZiI6MTcyNTc5MDczOCwicGF0aCI6Ii8xMDMzNTMwODQvMzY1MzY2NzE3LTE0N2Q4ODJlLWQ4MGUtNDhiMy1hNTgxLThhYTJiMTU1MDYzYi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTA4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkwOFQxMDE4NThaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT02MWIwMzU3Yjk1ZjgwMTJmM2U0NGQzN2ExNGNiNWRmMjQxYWQ4Y2ExNGY2Mjc2YmYxNzk4NGU1NGM1MmRmODVjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.9rVVwD7188TVnm86I26j1gZRodmcNiNlzIKOGEkMGTc) ![image](https://private-user-images.githubusercontent.com/103353084/365366838-402835ab-65bc-47d5-b902-00c2e9687769.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjU3OTEwMzgsIm5iZiI6MTcyNTc5MDczOCwicGF0aCI6Ii8xMDMzNTMwODQvMzY1MzY2ODM4LTQwMjgzNWFiLTY1YmMtNDdkNS1iOTAyLTAwYzJlOTY4Nzc2OS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTA4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkwOFQxMDE4NThaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1kMTQ0ZTJjYjY4ZDM3Y2VkZTcwZjBjM2U4OGExZDk0ODIxNWZlZDYyMDg3NzIzYThmZjJlOGFkZWE3ZDdhODMwJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.ftxR57c9UpxgC2MhhpbFa_4P0WQhNHWBAPnNqfJhxPk)



---

_Comment by @WangZhongDian on 2024-09-09 08:50_

@charliermarsh  I found that other packages can be downloaded from the cache, but Torch cannot. I hope this can be improved

---

_Closed by @WangZhongDian on 2024-09-10 02:12_

---
