```yaml
number: 11078
title: "Version discovery with untrusted `.venv` Python binary"
type: issue
state: closed
author: RoloEdits
labels:
  - question
  - security
assignees: []
created_at: 2025-01-29T19:05:53Z
updated_at: 2025-01-29T20:20:04Z
url: https://github.com/astral-sh/uv/issues/11078
synced_at: 2026-01-12T16:00:27Z
```

# Version discovery with untrusted `.venv` Python binary

---

_@RoloEdits_

### Question

While submitting https://github.com/starship/starship/pull/6523 to add version detection for `uv` managed python installations, worry came up for potential issues with untrusted python binaries being executed with no interaction. Is there any mitigation `uv` does against such untrusted `.venv` python binaries? And if not, is there ever a safe way this could be done?

### Platform

Windows 11

### Version

uv 0.5.25 (9c07c3fc5 2025-01-28)

---

_Label `question` added by @RoloEdits on 2025-01-29 19:05_

---

_Comment by @zanieb on 2025-01-29 19:49_

We have no concept of trusted or untrusted Python binaries. If there are binaries on your system that are executable, uv may invoke them while sniffing for Python interpreters. I don't know what the threat model is here, like... you have a malicious binary on your machine already?

You can use `--python <path>` to explicitly request a Python binary. I think that's my best recommendation?

Are there ways you think we could verify the safety of arbitrary Python binaries?

---

_Comment by @RoloEdits on 2025-01-29 20:05_

I thought that would be the case. Just wanted to know if there was anything in this scope that I was not accounting for. Any validation would need checksums I would venture. But given that `.venv` should not be in source control, and any executables in there should be install from the user, which should, in theory, be ok. Always good to double check though. Thanks for the response!

---

_Label `security` added by @zanieb on 2025-01-29 20:19_

---

_Comment by @zanieb on 2025-01-29 20:20_

Yeah sorry there's not a better answer. Especially on Windows, where the binaries are _copied_ into virtual environments it's hard to verify that it's some trusted system interpreter without doing a fair bit of work.

---

_Closed by @zanieb on 2025-01-29 20:20_

---
