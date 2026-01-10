---
number: 8368
title: "Choosing the ``license`` content for winget-pkgs manifest."
type: issue
state: closed
author: FishAlchemist
labels:
  - question
  - releases
assignees: []
created_at: 2024-10-19T16:37:04Z
updated_at: 2024-10-22T02:36:15Z
url: https://github.com/astral-sh/uv/issues/8368
synced_at: 2026-01-10T01:24:27Z
---

# Choosing the ``license`` content for winget-pkgs manifest.

---

_Issue opened by @FishAlchemist on 2024-10-19 16:37_

In the past, many UV version in winget-pkgs listed Apache-2.0 as the license but provided a link to the MIT license. A small portion listed MIT and provided a link to the MIT license as well.
https://github.com/microsoft/winget-pkgs/pulls?q=is%3Apr+New+version%3A+astral-sh.uv+is%3Aclosed

![image](https://github.com/user-attachments/assets/91762fcb-f510-4f32-b330-2ae46e67952a)
Although the README states that either license should be acceptable, considering that winget-pkgs is a software distribution channel, I'd like to ask which description would be more appropriate.
* **Option 1** (Automatically generated output from [Komac](https://github.com/russellbanks/Komac) typically looks like this.)
```yaml
License: Apache-2.0
LicenseUrl: https://github.com/astral-sh/uv/blob/main/LICENSE-MIT
Copyright: Copyright (c) 2023 Astral Software Inc.
CopyrightUrl: https://github.com/astral-sh/uv/blob/main/LICENSE-MIT
```
* **Option 2**
```yaml
License: MIT License
LicenseUrl: https://github.com/astral-sh/uv/blob/main/LICENSE-MIT
Copyright: Copyright (c) 2023 Astral Software Inc.
CopyrightUrl: https://github.com/astral-sh/uv/blob/main/LICENSE-MIT
```
* **Option 3** **(The changes I want to make.)**
Taking UV version 0.4.24, which is already on winget-pkgs, as an example.
```yaml
License: Either the Apache-2.0 License, or the MIT License
LicenseUrl: https://github.com/astral-sh/uv/tree/0.4.24?tab=readme-ov-file#license
Copyright: Copyright (c) 2023 Astral Software Inc.
CopyrightUrl: https://github.com/astral-sh/uv/tree/0.4.24?tab=readme-ov-file#license
```

Option 1 and 2 are cases that have already been merged into winget-pkgs.
Option 3 has passed local ``winget validate`` validation , but has never been submitted to winget-pkgs. However, this is a change I want to make, although I'm unsure if it's meaningful.
Or more suitable option 4?

Edit:
In https://github.com/microsoft/winget-pkgs/pull/184787, option (3) was merged into winget-pkgs .

---

_Comment by @zanieb on 2024-10-19 18:00_

Option (3) seems the correct if they will accept it. It might be problematic for downstream consumers of that API though? I'd see what they recommend upstream.

---

_Comment by @FishAlchemist on 2024-10-19 18:07_

> Option (3) seems the correct if they will accept it. It might be problematic for downstream consumers of that API though? I'd see what they recommend upstream.

If you run ``winget show --id astral-sh.uv --version 0.4.24``, you'll get the file's contents. But to find out that UV has a dual license, you'll still need to look it up on GitHub.
https://github.com/microsoft/winget-pkgs/blob/master/manifests/a/astral-sh/uv/0.4.24/astral-sh.uv.locale.en-US.yaml

---

_Label `releases` added by @zanieb on 2024-10-19 18:25_

---

_Label `question` added by @zanieb on 2024-10-19 18:25_

---

_Referenced in [microsoft/winget-pkgs#184787](../../microsoft/winget-pkgs/pulls/184787.md) on 2024-10-21 16:35_

---

_Comment by @FishAlchemist on 2024-10-22 02:18_

Option 3 of UV 0.4.25 has been merged into winget-pkgs (https://github.com/microsoft/winget-pkgs/pull/184787). 
(Although I linked directly to the README.)
This change will directly write two acceptable licenses in winstall.
![image](https://github.com/user-attachments/assets/d3aa11a5-3256-476e-a461-3df5d3af6799)


---

_Comment by @zanieb on 2024-10-22 02:36_

Thanks

---

_Closed by @zanieb on 2024-10-22 02:36_

---

_Referenced in [microsoft/winget-pkgs#188802](../../microsoft/winget-pkgs/pulls/188802.md) on 2024-11-05 01:55_

---
