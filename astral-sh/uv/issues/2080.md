```yaml
number: 2080
title: Local version identifiers resolved incorrectly
type: issue
state: closed
author: rodgar-nvkz
labels:
  - duplicate
assignees: []
created_at: 2024-02-29T13:42:55Z
updated_at: 2024-03-16T14:25:56Z
url: https://github.com/astral-sh/uv/issues/2080
synced_at: 2026-01-10T05:40:32Z
```

# Local version identifiers resolved incorrectly

---

_Issue opened by @rodgar-nvkz on 2024-02-29 13:42_

Hello, 
I'm having trouble installing specific versions of torch and torchvision with CUDA support using uv. I'm trying to install torch==1.12.1+cu113 and torchvision==0.13.1+cu113 but I'm encountering dependency resolution issues. I couldn't find an existing issue that addresses this concern, so I'm opening a new one.

Here's the command I'm using:
```uv pip install torch==1.12.1+cu113 torchvision==0.13.1+cu113 --index-url https://download.pytorch.org/whl/cu113```

And here's the error message I'm getting:
```
  × No solution found when resolving dependencies:
  ╰─▶ Because torchvision==0.13.1+cu113 depends on torch==1.12.1 and you require torch==1.12.1+cu113, we can conclude that you require==0a0.dev0 and torchvision==0.13.1+cu113 are incompatible.
      And because you require torchvision==0.13.1+cu113, we can conclude that the requirements are unsatisfiable.
```

A verbose stack trace can be found [here](https://gist.github.com/rodgar-nvkz/a2913abeb2831164ebefd61c89bf58ce). 

It seems like the +cu113 part in torch==1.12.1+cu113 is not treated as a local version identifier by uv, but rather as a version label. According to [Python Packaging User Guide](https://packaging.python.org/en/latest/specifications/version-specifiers/#local-version-identifiers), local version labels are used to denote project-specific versions.
Thank you! 

---

_Label `duplicate` added by @BurntSushi on 2024-02-29 13:54_

---

_Comment by @BurntSushi on 2024-02-29 13:55_

I believe this is a duplicate of #1855. And note the work-around linked from there: https://github.com/astral-sh/uv/issues/1497#issuecomment-1959429864

---

_Closed by @BurntSushi on 2024-02-29 13:55_

---

_Closed by @charliermarsh on 2024-03-16 14:24_

---

_Comment by @charliermarsh on 2024-03-16 14:25_

This will be supported in the next release via https://github.com/astral-sh/uv/pull/2430.

---
