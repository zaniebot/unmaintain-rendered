```yaml
number: 12137
title: "build-backend: Allow overriding module names for editable builds"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
assignees: []
merged: true
base: main
head: override-module-name-editable
created_at: 2025-03-12T16:31:43Z
updated_at: 2025-03-15T17:15:27Z
url: https://github.com/astral-sh/uv/pull/12137
synced_at: 2026-01-10T11:10:39Z
```

# build-backend: Allow overriding module names for editable builds

---

_Pull request opened by @blueraft on 2025-03-12 16:31_

## Summary

This PR enables module name overrides for editable installs. 

Builds upon https://github.com/astral-sh/uv/pull/11884.  The `tool.uv.build-backend.module-name` option is now respected during editable build processes.

## Test Plan

Added a test.

---

_@charliermarsh approved on 2025-03-14 01:14_

Thanks!

---

_Merged by @charliermarsh on 2025-03-14 01:27_

---

_Closed by @charliermarsh on 2025-03-14 01:27_

---

_Label `enhancement` added by @charliermarsh on 2025-03-14 01:27_

---

_Comment by @abedhammoud on 2025-03-15 06:02_

UV A great addition to the python echo system. thanks. 

I am having difficulties getting this to work for my project. I keep getting the error:

```code
>> Installing tool from current dir: /cygdrive/c/Users/abed/dev/GitHub/alant-otp/
Resolved 132 packages in 3.63s
  x Failed to build `alant-otp @ file:///C:/Users/abed/dev/GitHub/alant-otp` 
Expected a Python module with an `__init__.py` at: `src\alant_otp`     
  ```
  
  When I do:  v tool install --force --force-reinstall -e .[dev]
  
  with the structure of the project changes to alant-otp/src/otp 
  
  and my pyproject file containing:
  
 [tool.uv.build-backend]
module-root = "src"
module-name = "otp"

using the latest version of uv and py3.13


---

_Comment by @charliermarsh on 2025-03-15 14:52_

This change hasn’t shipped yet.

---

_Comment by @abedhammoud on 2025-03-15 17:15_

Thanks.

Sent from Outlook for iOS<https://aka.ms/o0ukef>
________________________________
From: Charlie Marsh ***@***.***>
Sent: Saturday, March 15, 2025 3:52:51 PM
To: astral-sh/uv ***@***.***>
Cc: Abed Hammoud ***@***.***>; Comment ***@***.***>
Subject: Re: [astral-sh/uv] build-backend: Allow overriding module names for editable builds (PR #12137)


This change hasn’t shipped yet.

—
Reply to this email directly, view it on GitHub<https://github.com/astral-sh/uv/pull/12137#issuecomment-2726698397>, or unsubscribe<https://github.com/notifications/unsubscribe-auth/AOBITHG62NOQNT6OZ5V2VV32UQ5EHAVCNFSM6AAAAABY4BOPAOVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDOMRWGY4TQMZZG4>.
You are receiving this because you commented.Message ID: ***@***.***>

[charliermarsh]charliermarsh left a comment (astral-sh/uv#12137)<https://github.com/astral-sh/uv/pull/12137#issuecomment-2726698397>

This change hasn’t shipped yet.

—
Reply to this email directly, view it on GitHub<https://github.com/astral-sh/uv/pull/12137#issuecomment-2726698397>, or unsubscribe<https://github.com/notifications/unsubscribe-auth/AOBITHG62NOQNT6OZ5V2VV32UQ5EHAVCNFSM6AAAAABY4BOPAOVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDOMRWGY4TQMZZG4>.
You are receiving this because you commented.Message ID: ***@***.***>


---
