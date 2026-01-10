---
number: 12578
title: "uv run python in vllm dir failed `No solution found when resolving dependencies for split`"
type: issue
state: open
author: yihong0618
labels:
  - question
assignees: []
created_at: 2025-03-31T08:50:44Z
updated_at: 2025-04-02T14:15:37Z
url: https://github.com/astral-sh/uv/issues/12578
synced_at: 2026-01-10T01:25:21Z
---

# uv run python in vllm dir failed `No solution found when resolving dependencies for split`

---

_Issue opened by @yihong0618 on 2025-03-31 08:50_

error:

```
⠴ Resolving dependencies...                                                                                                                                          × No solution found when resolving dependencies for split (python_full_version >= '3.12' and platform_machine == 'x86_64' and sys_platform == 'darwin'):
  ╰─▶ Because there is no version of torch{platform_machine == 'x86_64'}==2.6.0+cpu and your project depends on torch{platform_machine == 'x86_64'}==2.6.0+cpu,
      we can conclude that your project's requirements are unsatisfiable.
      And because your project requires vllm[audio], we can conclude that your project's requirements are unsatisfiable.
```

OS macOS m3 uv and vllm both latest

reproduce:

1. clone vllm && cd vllm
2. uv python pin 3.12
3. uv pip install -r requirements/cpu.txt     
4. uv pip install -e .

then `uv run python failed` with the error message

debug:

cat requirements/cpu.txt

```
-r common.txt

# Dependencies for CPUs
torch==2.6.0+cpu; platform_machine == "x86_64"
torch==2.6.0; platform_system == "Darwin"
torch==2.6.0; platform_machine == "ppc64le" or platform_machine == "aarch64"
torch==2.7.0.dev20250304; platform_machine == "s390x"

# required for the image processor of minicpm-o-2_6, this must be updated alongside torch
torchaudio; platform_machine != "ppc64le" and platform_machine != "s390x"
torchaudio==2.6.0; platform_machine == "ppc64le"

# required for the image processor of phi3v, this must be updated alongside torch
torchvision; platform_machine != "ppc64le"  and platform_machine != "s390x"
torchvision==0.21.0; platform_machine == "ppc64le"
datasets # for benchmark scripts
```

only keep torch==2.6.0; platform_system == "Darwin" works

source .venv/bin/activate 
then 
python -c 'import vllm' works


---

_Comment by @charliermarsh on 2025-03-31 13:03_

This looks right -- those versions of PyTorch don't exist on PyPI. How are you configuring your index?

---

_Label `question` added by @charliermarsh on 2025-03-31 13:03_

---

_Comment by @yihong0618 on 2025-03-31 13:47_

> This looks right -- those versions of PyTorch don't exist on PyPI. How are you configuring your index?

seems the problem is from vllm side so closing it, thanks

---

_Closed by @yihong0618 on 2025-03-31 13:47_

---

_Comment by @yihong0618 on 2025-04-02 04:54_

@charliermarsh I added it to dependency_links in setup.py but not work, is uv support `--extra-index-url` with dependency_links in setup.py?

![Image](https://github.com/user-attachments/assets/b8f8bbc3-8cda-4cef-855e-f180fe61fa7e)

---

_Reopened by @yihong0618 on 2025-04-02 05:39_

---

_Comment by @zanieb on 2025-04-02 14:15_

We don't support `setup.py` specific fields no, those are all run via `setuptools` and don't use uv configuration.

---

_Referenced in [vllm-project/vllm#15985](../../vllm-project/vllm/issues/15985.md) on 2025-04-03 03:36_

---

_Referenced in [vllm-project/vllm#15986](../../vllm-project/vllm/pulls/15986.md) on 2025-04-03 03:46_

---
