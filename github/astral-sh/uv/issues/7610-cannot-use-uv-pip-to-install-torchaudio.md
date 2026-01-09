---
number: 7610
title: "Cannot use `uv pip` to install torchaudio "
type: issue
state: closed
author: JavaZeroo
labels:
  - question
assignees: []
created_at: 2024-09-21T07:01:26Z
updated_at: 2024-09-24T07:22:24Z
url: https://github.com/astral-sh/uv/issues/7610
synced_at: 2026-01-07T13:12:17-06:00
---

# Cannot use `uv pip` to install torchaudio 

---

_Issue opened by @JavaZeroo on 2024-09-21 07:01_

# Running

 `uv pip install torchaudio --index-url https://download.pytorch.org/whl/cu124`

# uv version

`uv 0.4.14` on linux.

# output

```
🌀 ~  ai 🕙[ 14:58:12 ]
💫uv pip install torchaudio --index-url https://download.pytorch.org/whl/cu124
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of torchaudio are available:
          torchaudio==0.2
          torchaudio==0.3.0
          torchaudio==0.3.1
          torchaudio==0.3.2
          torchaudio==0.4.0
          torchaudio==0.5.0
          torchaudio==0.5.1
          torchaudio==0.6.0
          torchaudio==0.7.0
          torchaudio==0.7.1
          torchaudio==0.7.2
          torchaudio==0.8.0
          torchaudio==0.8.1
          torchaudio==0.9.0
          torchaudio==0.9.1
          torchaudio==0.10.0
          torchaudio==0.10.1
          torchaudio==0.10.2
          torchaudio==0.11.0
          torchaudio==0.12.0
          torchaudio==0.12.1
          torchaudio==0.13.0
          torchaudio==0.13.1
          torchaudio==2.0.0
          torchaudio==2.0.1
          torchaudio==2.0.2
          torchaudio==2.1.0
          torchaudio==2.1.1
          torchaudio==2.1.2
          torchaudio==2.2.0
          torchaudio==2.4.0+cu124
          torchaudio==2.4.1+cu124
      and torchaudio<=0.10.2 has no wheels with a matching Python ABI tag, we can conclude that torchaudio<0.3.0 cannot be used.
      And because torchaudio>=0.11.0,<=2.2.0 has no wheels with a matching platform tag, we can conclude that torchaudio<0.12.0 cannot be used. (1)

      Because torch==2.4.0 has no wheels with a matching platform tag and torchaudio==2.4.0+cu124 depends on torch==2.4.0, we can conclude that torchaudio==2.4.0+cu124 cannot
      be used.
      And because we know from (1) that torchaudio<0.12.0 cannot be used, we can conclude that torchaudio<2.4.1+cu124 cannot be used. (2)

      Because torch==2.4.1 has no wheels with a matching platform tag and torchaudio==2.4.1+cu124 depends on torch==2.4.1, we can conclude that torchaudio==2.4.1+cu124 cannot
      be used.
      And because we know from (2) that torchaudio<2.4.1+cu124 cannot be used, we can conclude that all versions of torchaudio cannot be used.
      And because you require torchaudio, we can conclude that your requirements are unsatisfiable.
```
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @charliermarsh on 2024-09-21 12:37_

Try: `uv pip install torchaudio==2.4.0+cu124 torch==2.4.0+cu124 --index-url https://download.pytorch.org/whl/cu124`.

More here: https://docs.astral.sh/uv/pip/compatibility/#local-version-identifiers.

---

_Label `question` added by @charliermarsh on 2024-09-21 12:37_

---

_Comment by @JavaZeroo on 2024-09-24 07:22_

> Try: `uv pip install torchaudio==2.4.0+cu124 torch==2.4.0+cu124 --index-url https://download.pytorch.org/whl/cu124`.
> 
> More here: https://docs.astral.sh/uv/pip/compatibility/#local-version-identifiers.

thats helpful, thx

---

_Closed by @JavaZeroo on 2024-09-24 07:22_

---
