```yaml
number: 2037
title: Error while installing pytorch with uv
type: issue
state: closed
author: Tanmaypatil123
labels:
  - question
assignees: []
created_at: 2024-02-28T08:25:56Z
updated_at: 2024-04-04T03:55:11Z
url: https://github.com/astral-sh/uv/issues/2037
synced_at: 2026-01-12T15:58:34Z
```

# Error while installing pytorch with uv

---

_@Tanmaypatil123_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with UV.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current Uv platform.
* The current uv version (`uv --version`).
-->

I am facing issues in installing Python using uv package installer.

I am executing `uv pip install --pre torch torchvision torchaudio --force-reinstall --index-url https://download.pytorch.org/whl/nightly/cu118`.

I was not able to find related documentation so any help would be appreciated. 

---

_Comment by @konstin on 2024-02-28 09:22_

Could you post the error message you got?

---

_Comment by @jensens on 2024-02-28 13:35_

Probably because of `--pre` - I just stumbled over the same issue. Same in uv is `--prerelease=allow`. Another thing to document as pip incompatibility (see also #2023)
Question is it would not be better to keep `--pre` working as it is use a lot with pip.

---

_Comment by @zanieb on 2024-02-28 18:32_

I created an issue for that at #2047 

Is your issue resolved with prereleases allowed?

---

_Label `question` added by @zanieb on 2024-02-28 18:32_

---

_Comment by @charliermarsh on 2024-04-04 03:55_

Closing for now, as I suspect this is resolved by the docs in https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#local-version-identifiers.

---

_Closed by @charliermarsh on 2024-04-04 03:55_

---
