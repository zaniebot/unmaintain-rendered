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
updated_at: 2026-01-21T20:20:57Z
url: https://github.com/astral-sh/uv/issues/17644
synced_at: 2026-01-21T21:17:27Z
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

_Comment by @zanieb on 2026-01-21 20:20_

How would we know that an argument after `<command>` is a target for uv? I'm not sure we want to start scanning for scripts in arbitrary command arguments.

I think the current suggestion would be to do `uv run --with-requirements-from train.py torchrun train.py`, which is a bit verbose. We've also considered something like `uv run --wrapper torchrun -- train.py`. See #7032

---
