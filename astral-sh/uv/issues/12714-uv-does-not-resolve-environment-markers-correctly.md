```yaml
number: 12714
title: UV does not resolve environment markers correctly
type: issue
state: closed
author: Vdaleke
labels:
  - question
assignees: []
created_at: 2025-04-07T13:26:01Z
updated_at: 2025-04-07T14:13:34Z
url: https://github.com/astral-sh/uv/issues/12714
synced_at: 2026-01-12T16:01:11Z
```

# UV does not resolve environment markers correctly

---

_@Vdaleke_

### Summary

My project depends on "docling>=2.27.0,<3.0.0" and "tokenizers>=0.20.2" for Python3.13 support.
When `uv lock` I get an error:
```
➜  docling-vision-ocr uv lock
  × No solution found when resolving dependencies for split (python_full_version >= '3.12' and platform_machine == 'x86_64' and sys_platform == 'darwin'):
  ╰─▶ Because transformers>=4.42.0,<=4.42.4 depends on tokenizers>=0.19,<0.20 and only the following versions of transformers{platform_machine == 'x86_64' and
      sys_platform == 'darwin'} are available:
          transformers{platform_machine == 'x86_64' and sys_platform == 'darwin'}<=4.42.0
          transformers{platform_machine == 'x86_64' and sys_platform == 'darwin'}==4.42.1
          transformers{platform_machine == 'x86_64' and sys_platform == 'darwin'}==4.42.2
          transformers{platform_machine == 'x86_64' and sys_platform == 'darwin'}==4.42.3
          transformers{platform_machine == 'x86_64' and sys_platform == 'darwin'}==4.42.4
          transformers{platform_machine == 'x86_64' and sys_platform == 'darwin'}>4.43.0
      we can conclude that transformers{platform_machine == 'x86_64' and sys_platform == 'darwin'}>=4.42.0,<4.43.0 depends on tokenizers>=0.19,<0.20.
      And because docling-ibm-models>=3.4.0 depends on transformers{platform_machine == 'x86_64' and sys_platform == 'darwin'}>=4.42.0,<4.43.0, we can conclude that
      docling-ibm-models>=3.4.0 depends on tokenizers>=0.19,<0.20.
      And because only the following versions of docling-ibm-models are available:
          docling-ibm-models<=3.4.0
          docling-ibm-models==3.4.1
      and docling>=2.27.0 depends on docling-ibm-models>=3.4.0, we can conclude that docling>=2.27.0 depends on tokenizers>=0.19,<0.20.
      And because only the following versions of docling are available:
          docling<=2.27.0
          docling==2.28.0
          docling==2.28.1
          docling==2.28.2
          docling==2.28.3
          docling==2.28.4
      and your project depends on docling>=2.27.0, we can conclude that your project depends on tokenizers>=0.19,<0.20.
      And because your project depends on tokenizers>=0.20.2 and your project requires docling-vision-ocr[dev], we can conclude that your project's requirements are
      unsatisfiable.
```
If you look at the markers for docling-ibm-models:
```
➜  python curl -s https://pypi.org/pypi/docling-ibm-models/json | jq '.info.requires_dist'
[
  "torch<3.0.0,>=2.2.2",
  "torchvision<1,>=0",
  "transformers<5.0.0,>=4.42.0; sys_platform != \"darwin\" or platform_machine != \"x86_64\"",
  "transformers<4.43.0,>=4.42.0; sys_platform == \"darwin\" and platform_machine == \"x86_64\"",
  "numpy<3.0.0,>=1.24.4; sys_platform != \"darwin\" or platform_machine != \"x86_64\"",
  "numpy<2.0.0,>=1.24.4; sys_platform == \"darwin\" and platform_machine == \"x86_64\"",
  "jsonlines<4.0.0,>=3.1.0",
  "Pillow<12.0.0,>=10.0.0",
  "tqdm<5.0.0,>=4.64.0",
  "opencv-python-headless<5.0.0.0,>=4.6.0.66",
  "huggingface_hub<1,>=0.23",
  "safetensors[torch]<1,>=0.4.3",
  "pydantic<3.0.0,>=2.0.0",
  "docling-core<3.0.0,>=2.19.0"
]
```
then you can see that the second marker is selected instead of the first. `{platform_machine == 'x86_64' and sys_platform == 'darwin'}` is not correct since I work on ARM, and `sys_platform != \"darwin\" or platform_machine != \"x86_64\` is OK.  




### Platform

Darwin 24.3.0 arm64

### Version

uv 0.6.12 (e4e03833f 2025-04-02)

### Python version

Python 3.13.2

---

_Label `bug` added by @Vdaleke on 2025-04-07 13:26_

---

_Comment by @Vdaleke on 2025-04-07 13:28_

Hello @zanieb, I saw your answers in a similar issue #12678, can you take a look at this problem?

---

_Comment by @konstin on 2025-04-07 13:48_

`uv lock` creates an universal lockfile, which includes the `python_full_version >= '3.12' and platform_machine == 'x86_64' and sys_platform == 'darwin'` environment that fails to resolve here. If you want to only solve for specific environments, you can set `tool.uv.environments` (https://docs.astral.sh/uv/reference/settings/#environments).

---

_Label `bug` removed by @konstin on 2025-04-07 13:49_

---

_Label `question` added by @konstin on 2025-04-07 13:49_

---

_Comment by @Vdaleke on 2025-04-07 14:13_

>  If you want to only solve for specific environments, you can set `tool.uv.environments` (https://docs.astral.sh/uv/reference/settings/#environments).

Thank you very much for the quick reply! With this configuration:
```
[tool.uv]
environments = ["sys_platform != 'darwin' or platform_machine != 'x86_64'"]
```
 it works as I expected!


---

_Closed by @Vdaleke on 2025-04-07 14:13_

---
