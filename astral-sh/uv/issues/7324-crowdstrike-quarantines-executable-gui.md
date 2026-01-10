---
number: 7324
title: Crowdstrike quarantines executable GUI entrypoints upon installation on Windows
type: issue
state: open
author: johannesloibl
labels:
  - windows
assignees: []
created_at: 2024-09-12T08:07:43Z
updated_at: 2024-12-12T18:11:28Z
url: https://github.com/astral-sh/uv/issues/7324
synced_at: 2026-01-10T01:24:13Z
---

# Crowdstrike quarantines executable GUI entrypoints upon installation on Windows

---

_Issue opened by @johannesloibl on 2024-09-12 08:07_

Hey,

don't know if this is the right place, but my company recently installed Crowdstrike (yeah, that one)
and it is now messing with me after switching to awesome UV.

For only **some** of our internal Python packages, Crowdstrike is quarantining the EXE that is created by UV (because of defined gui application entrypoints of the library),
when i try installing it via ``uv tool install`` or creating a venv and installing the library using ``uv pip install``.

Why i'm creating this issue here? Well, if i'm using ``pip`` and ``pipx`` to install,
everything works fine.
Maybe Crowdstrike has a problem with the ``uv-trampoline`` bootstrap code? This seems to be the only difference between executables from UV and PIPX, or am i wrong?

It seems to be only an issue for entrypoints defined in `[project.gui-scripts]`, entrypoints from `[project.scripts]` are not quarantined.

![image](https://github.com/user-attachments/assets/8187aaba-1c75-4e81-80bf-82b15b1b5148)

UV version: uv 0.4.9 (77d278f68 2024-09-10)
Windows 10

I could make a small reproducible Python project example that triggers the quarantine and attached it: 
[cs_false_positive.zip](https://github.com/user-attachments/files/16976061/cs_false_positive.zip)
Just unpack and execute ``trigger_crowdstrike.ps1``. This will create a venv and install the project. Crowdstrike will then delete ``.venv\Scripts\hello-app.exe`` but ignore ``.venv\Scripts\hello-cli.exe``.


---

_Renamed from "Crowdstrike quarantines SOME executable entrypoints upon installation on Windows" to "Crowdstrike quarantines executable GUI entrypoints upon installation on Windows" by @johannesloibl on 2024-09-12 08:41_

---

_Comment by @johannesloibl on 2024-09-16 12:50_

@ofek FYI, your PyApps are also affected by this (UV + GUI), just got the confirmation of a colleague.

---

_Comment by @zanieb on 2024-09-16 13:07_

Thanks for the report.

Did you also report this as a false-positive to Crowdstrike? I'd love to hear what they say too. We're not doing anything malicious :)

---

_Label `windows` added by @zanieb on 2024-09-16 13:07_

---

_Comment by @johannesloibl on 2024-09-16 13:40_

Yes our CyberSec team is reporting it to them, but i probably don't get any information i could share with you. Since they are doing a lot of scanning based on ML models, this can take some time until they updated to model to not flag the executables anymore, if they do this at all...

---

_Comment by @magoocas on 2024-11-07 18:33_

Suffering from the same problem. Our cybersec team has also submitted a report on the false-positive to Crowdstrike. 

My workaround in the meantime is to use .cmd "shebangs" instead of the trampoline feature.

[cs_workaround.patch](https://github.com/user-attachments/files/17668666/cs_workaround.patch)

Works for my use case, YMMV!

---

_Comment by @axel-kah on 2024-12-12 18:11_

Just wanted to report that this is still a blocking issue (in enterprise environments anyway) since crowdstrike has not adapted their scanners yet.
With `uv` being used under the hood of more and more tools, the impact is growing as well 😐 

---
