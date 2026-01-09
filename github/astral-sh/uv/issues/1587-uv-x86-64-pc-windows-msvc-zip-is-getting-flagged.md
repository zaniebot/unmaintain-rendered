---
number: 1587
title: " uv-x86_64-pc-windows-msvc.zip is getting flagged by Windows Defender"
type: issue
state: closed
author: pfmoore
labels:
  - question
assignees: []
created_at: 2024-02-17T13:13:09Z
updated_at: 2024-02-18T20:29:48Z
url: https://github.com/astral-sh/uv/issues/1587
synced_at: 2026-01-07T13:12:16-06:00
---

#  uv-x86_64-pc-windows-msvc.zip is getting flagged by Windows Defender

---

_Issue opened by @pfmoore on 2024-02-17 13:13_

I tried to download the latest release, 0.1.3, specifically the file uv-x86_64-pc-windows-msvc.zip, but Windows Defender flagged it as malicious with the error

![image](https://github.com/astral-sh/uv/assets/1110419/12618a47-cf9a-4c29-bbc5-dfd9a84fdd6f)

(sorry for the screenshot, the dialog box won't let me copy the text out 🙁)

I assume this is a false positive, but I'm reluctant to tell the virus checker to simply ignore the issue. The 0.1.0 release didn't have this problem.

---

_Comment by @zanieb on 2024-02-17 19:36_

Sorry but there's usually nothing we can do about these false positives — we get these in Ruff too.

---

_Label `question` added by @zanieb on 2024-02-17 19:36_

---

_Comment by @pfmoore on 2024-02-17 19:38_

No worries, I didn't think so - I just wanted to let you know. I can wait for the next release (or build it myself - it's impressively straightforward to build 🙂)

---

_Closed by @pfmoore on 2024-02-17 19:39_

---

_Comment by @charliermarsh on 2024-02-17 22:31_

These are my worst enemy.

---

_Comment by @senden9 on 2024-02-18 07:41_

I got by the same windows defender update. I was using `uv` and from one moment to the next i got the message that the command `uv` is unknown. A few moments later windows defender proudly notified my that it detected and quarantined `uv.exe` for containing `Trojan:Script/Wacatac.B!ml`. I did come here and write that comment to have the name of the falsely detected threat somewhere in the issue tracker.

---

_Comment by @senden9 on 2024-02-18 07:52_

I reported that false positive under https://www.microsoft.com/en-us/wdsi/filesubmission as Customer.
Maybe that will help other users of `uv` to get avoid a false-positive in the future.

@zanieb: Regarding the "nothing we can do". On the same page one can also report false positives as a developer of a software. I do not know if this then get a higher priority by Microsoft. 

If Microsoft updates my case in a timely manner I will keep updating here even if that issue is already closed. If this is unwanted please say so.

---

_Comment by @senden9 on 2024-02-18 20:29_

Update from the Analyst at Microsoft:

> At this time, the submitted files do not meet our criteria for malware or potentially unwanted applications. The detection has been removed. Please follow the steps below to clear cached detections and obtain the latest malware definitions.
> 
> 1. Open command prompt as administrator and change directory to c:\Program Files\Windows Defender
> 2. Run “MpCmdRun.exe -removedefinitions -dynamicsignatures”
> 3. Run "MpCmdRun.exe -SignatureUpdate"
> 
> Alternatively, the latest definition is available for download here: https://docs.microsoft.com/microsoft-365/security/defender-endpoint/manage-updates-baselines-microsoft-defender-antivirus
> 
> Thank you for contacting Microsoft.
> 
So as far as I understand `uv` should no longer trigger a Microsoft Defender quarantine with the latest update 🎉 

---

_Referenced in [astral-sh/uv#4300](../../astral-sh/uv/issues/4300.md) on 2024-06-13 12:39_

---

_Referenced in [microsoft/winget-pkgs#157541](../../microsoft/winget-pkgs/pulls/157541.md) on 2024-06-13 13:10_

---

_Referenced in [posit-dev/air#272](../../posit-dev/air/issues/272.md) on 2025-03-03 22:38_

---
