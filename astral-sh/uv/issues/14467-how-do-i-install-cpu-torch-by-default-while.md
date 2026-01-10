---
number: 14467
title: How do I install CPU torch by default while allowing GPU torch?
type: issue
state: closed
author: CookieHCl
labels:
  - question
assignees: []
created_at: 2025-07-05T16:22:14Z
updated_at: 2025-07-08T09:57:35Z
url: https://github.com/astral-sh/uv/issues/14467
synced_at: 2026-01-10T01:25:45Z
---

# How do I install CPU torch by default while allowing GPU torch?

---

_Issue opened by @CookieHCl on 2025-07-05 16:22_

### Question

I want to install CPU torch by default, (e.g. `uv run`) but allow using GPU torch for users. (e.g. `uv run --extra cu124`)

This is my pyproject.toml. (I'm using Python 3.11. i.e. .python-version = 3.11)

```toml
[project]
name = "r-vectors-visualization"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
    "huggingface-hub[hf-xet]==0.33.2",
    "transformers[torch]==4.53.1",
    "torch==2.6.0",
    "torchaudio==2.6.0",
    "torchvision==0.21.0",
]

[project.optional-dependencies]
cu124 = [
    "torch==2.6.0",
    "torchaudio==2.6.0",
    "torchvision==0.21.0",
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", marker = "extra != 'cu124'" },
  { index = "pytorch-cu124", marker = "extra == 'cu124'" },
]
torchaudio = [
  { index = "pytorch-cpu", marker = "extra != 'cu124'" },
  { index = "pytorch-cu124", marker = "extra == 'cu124'" },
]
torchvision = [
  { index = "pytorch-cpu", marker = "extra != 'cu124'" },
  { index = "pytorch-cu124", marker = "extra == 'cu124'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

I expected that `uv run` will install CPU version of torch (2.6.0+cpu), and `uv run --extra cu124` will install GPU version of torch (2.6.0+cu124).

However, this toml always install GPU version of torch, and [uv.lock](https://gist.github.com/CookieHCl/c91913a20c676fe9600def8dc5fa3454) only contains GPU version of the torch.  
How should I fix the toml?

### Platform

Windows_NT 10.0 x86_64 MS/Windows

### Version

uv 0.7.18 (87e9ccfb9 2025-07-01)

---

_Label `question` added by @CookieHCl on 2025-07-05 16:22_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-07 14:46_

---

_Comment by @charliermarsh on 2025-07-07 19:37_

Hmm, we don't quite have perfect support for this. Per the [docs](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies), we'd typically recommend:

```toml
[project]
name = "r-vectors-visualization"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
  "huggingface-hub[hf-xet]==0.33.2",
  "transformers[torch]==4.53.1",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu124" },
  ],
]

[project.optional-dependencies]
cpu = [
  "torch==2.6.0",
  "torchaudio==2.6.0",
  "torchvision==0.21.0",
]
cu124 = [
  "torch==2.6.0",
  "torchaudio==2.6.0",
  "torchvision==0.21.0",
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchaudio = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

That differs from your request in that users need to specify `--extra cpu`.


---

_Comment by @CookieHCl on 2025-07-08 09:57_

Thank you for your reply!

---

_Closed by @CookieHCl on 2025-07-08 09:57_

---
