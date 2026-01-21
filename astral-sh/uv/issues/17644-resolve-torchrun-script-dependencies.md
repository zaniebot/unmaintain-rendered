```yaml
number: 17644
title: Resolve torchrun script dependencies
type: issue
state: open
author: lhoestq
labels:
  - enhancement
assignees: []
created_at: 2026-01-21T18:19:21Z
updated_at: 2026-01-21T18:20:57Z
url: https://github.com/astral-sh/uv/issues/17644
synced_at: 2026-01-21T19:05:09Z
```

# Resolve torchrun script dependencies

---

_@lhoestq_

### Summary

Hey I'm Quentin from Hugging Face :)

Training an AI model with multiple GPUs requires using the `torchrun` or `accelerate launch` command, followed by the python script containing the training code. For example

```bash
torchrun train.py
```

uv doesn't resolve dependencies of scripts passed after such commands. For example this doesn't resolve dependencies in train.py:

```bash
uv run torchrun train.py
```

I think it would be great for uv to resolve dependencies in this case

cc @davanstrien for viz

### Example

_No response_

---

_Label `enhancement` added by @lhoestq on 2026-01-21 18:19_

---
