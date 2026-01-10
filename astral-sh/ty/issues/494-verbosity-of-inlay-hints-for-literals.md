```yaml
number: 494
title: Verbosity of inlay hints for Literals
type: issue
state: closed
author: rushter
labels:
  - server
assignees: []
created_at: 2025-05-23T11:42:42Z
updated_at: 2025-11-10T21:56:26Z
url: https://github.com/astral-sh/ty/issues/494
synced_at: 2026-01-10T02:06:24Z
```

# Verbosity of inlay hints for Literals

---

_Issue opened by @rushter on 2025-05-23 11:42_

It would be nice to be able to tune the verbosity of inlay hints for literals.
Right now, the whole string is shown, which is usually not very useful, especially for very long strings.

<img width="1429" alt="Image" src="https://github.com/user-attachments/assets/204e15ca-5a9e-4b47-abd6-f5dac49a9a86" />

---

_Label `server` added by @MichaReiser on 2025-05-23 13:13_

---

_Label `wish` added by @MichaReiser on 2025-05-23 13:14_

---

_Comment by @MichaReiser on 2025-05-23 13:14_

Makes sense. I'mn not sure when we'll get to it but this is certainly too verbose to be useful.

What editor are you using? I believe some editors have built in truncation.

---

_Comment by @rushter on 2025-05-23 13:16_


> What editor are you using? I believe some editors have built in truncation.

Zed

---

_Comment by @sharkdp on 2025-05-23 13:55_

related discussion: https://github.com/astral-sh/ty/issues/472

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 12:10_

---

_Label `wish` removed by @MichaReiser on 2025-08-15 13:41_

---

_Assigned to @Gankra by @Gankra on 2025-08-20 13:21_

---

_Comment by @MichaReiser on 2025-10-15 12:26_

I believe pylance disables inlays for literals, or at least has an option for it.

We should also avoid rendering too long inlays as seen in [Disable type hints for single lines #1359](https://github.com/astral-sh/ty/issues/1359)

---

_Comment by @Gankra on 2025-10-31 16:12_

Basic support for this was actually implemented in https://github.com/astral-sh/ruff/pull/20928

<img width="942" height="41" alt="Image" src="https://github.com/user-attachments/assets/fc0f36b3-9e60-49eb-bd56-83cc11b6eca5" />

It's tuned a bit long for my tastes but that's something we can tweak.

---

_Comment by @MichaReiser on 2025-10-31 18:25_

Pylance also disables inlays for literals:

<img width="925" height="604" alt="Image" src="https://github.com/user-attachments/assets/c61ae6c5-554a-4755-8993-698f61f5e8b8" />

Which I think makes sense. It seems somewhat pointless to repeat the right hand side

---

_Closed by @Gankra on 2025-11-10 21:56_

---
