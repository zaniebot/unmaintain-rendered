---
number: 5681
title: No solution found when resolving dependencies
type: issue
state: closed
author: nguyenvulong
labels:
  - question
assignees: []
created_at: 2024-08-01T03:29:54Z
updated_at: 2024-08-01T07:33:33Z
url: https://github.com/astral-sh/uv/issues/5681
synced_at: 2026-01-07T13:12:17-06:00
---

# No solution found when resolving dependencies

---

_Issue opened by @nguyenvulong on 2024-08-01 03:29_

Requirement file: from https://github.com/ggerganov/llama.cpp

* This command worked fine
```
pip install llama.cpp/requirements/requirements-convert_hf_to_gguf.txt
```
* But adding `uv` caused the bug
```
uv pip install llama.cpp/requirements/requirements-convert_hf_to_gguf.txt
```

Error
```
× No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of numpy are available:
          numpy<1.26.4
          numpy>=1.27.dev0
      and you require numpy>=1.26.4,<1.27.dev0, we can conclude that the requirements are unsatisfiable.

      hint: numpy was requested with a pre-release marker (e.g., numpy>=1.26.4,<1.27.dev0), but pre-releases weren't enabled
      (try: `--prerelease=allow`)
```

when using `--prerelease=allow`
```
❯ uv  pip install -r llama.cpp/requirements/requirements-convert_hf_to_gguf.txt --prerelease=allow
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of numpy are available:
          numpy<1.26.4
          numpy>=1.27.dev0
      and you require numpy>=1.26.4,<1.27.dev0, we can conclude that the requirements are unsatisfiable.
```

* The current uv platform (I have no idea what "uv platform" is, so I provide the OS information instead)
```
Linux dev4-1 5.15.0-113-generic #123-Ubuntu
Description:	Ubuntu 22.04.4 LTS
Release:	22.04
Codename:	jammy

Python 3.12.4
pip 24.0
conda 24.1.2 (I tried both conda and venv, both failed)
```
* The current uv version (`uv --version`).
`uv 0.2.27`

---

_Comment by @zanieb on 2024-08-01 03:57_

Hi! This is a known difference from `pip`, please see the [relevant section in the compatibility guide](https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#pre-release-compatibility). As noted in the hint after the error, you need to include `--prerelease=allow`.

---

_Label `question` added by @zanieb on 2024-08-01 03:57_

---

_Comment by @nguyenvulong on 2024-08-01 04:34_

Actually I saw that hint and tried it before, it's the same error output.
Sorry I will update the question with this message as well

```
❯ uv  pip install -r llama.cpp/requirements/requirements-convert_hf_to_gguf.txt --prerelease=allow
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of numpy are available:
          numpy<1.26.4
          numpy>=1.27.dev0
      and you require numpy>=1.26.4,<1.27.dev0, we can conclude that the requirements are unsatisfiable.
```

---

_Comment by @blueraft on 2024-08-01 07:27_

The fix to this issue: https://github.com/astral-sh/uv/issues/4890#issuecomment-2218309488

---

_Comment by @nguyenvulong on 2024-08-01 07:33_

Thank you, it worked!

Full command.
```
uv  pip install -r llama.cpp/requirements/requirements-convert_hf_to_gguf.txt --prerelease=allow --index-strategy unsafe-first-match
```

I will close the issue now.

---

_Closed by @nguyenvulong on 2024-08-01 07:33_

---

_Referenced in [nguyenvulong/QA#84](../../nguyenvulong/QA/issues/84.md) on 2024-08-02 00:26_

---
