```yaml
number: 17352
title: Using extras to manually toggle dependency variants
type: issue
state: open
author: Oddball777
labels:
  - question
assignees: []
created_at: 2026-01-07T21:36:18Z
updated_at: 2026-01-08T00:37:11Z
url: https://github.com/astral-sh/uv/issues/17352
synced_at: 2026-01-10T03:11:36Z
```

# Using extras to manually toggle dependency variants

---

_Issue opened by @Oddball777 on 2026-01-07 21:36_

### Question

I have a project where I’d like to manually toggle between variants of a dependency using extras. Here is a minimal example of what I currently do:
```
[project]
name = "my-project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "jax>=0.8.0",
]

[project.optional-dependencies]
usecuda = [
    "jax[cuda]",
]
```

And then I run either `uv sync` or `uv sync --extras usecuda`, depending on whether I want CUDA support or not. This _seems to work_, but I haven’t found documentation that explicitly confirms this is a supported or intended pattern. Is uv explicitly designed to handle this kind of dependency override via extras?

I also wondered whether this same approach works for version switching, not just feature extras. For example:
```
[project]
name = "my-project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "numpy>=1.0.0,<2.0.0",
]

[project.optional-dependencies]
usenumpy2 = [
    "numpy>=2.0.0",
]
```

This feels related to prior discussions about environment markers (e.g. conditioning on architecture or platform, usually for PyTorch), but my use case is different:
- I do not want to condition on architecture (for example, I want to be able to install the non-CUDA variant on a CUDA-capable machine)
- The NumPy example shows that this is not inherently tied to hardware at all

Questions:
1. In the first case, does this actually toggle between `jax` and `jax[cuda]` in a way that is explicitly supported?
2. In the second example, does this actually allow toggling between numpy major versions, or is this relying on undefined / unintended resolver behavior?
3. Is there a more idiomatic or recommended uv approach for achieving this kind of manual dependency variant selection?

### Platform

macOS 26.2, Darwin 25.2.0 arm64 (main computer) and Linux 5.14.0-570.51.1.el9_6.aarch64+64k aarch64 GNU/Linux (CUDA-capable machine)

### Version

uv 0.9.17

---

_Label `question` added by @Oddball777 on 2026-01-07 21:36_

---

_Comment by @zanieb on 2026-01-07 23:13_

1. Yes, to my understanding that is supported.
2. I'm not sure you can declare dependencies in extra that conflict with the main project requirements. I'd expect that to fail based on Python standards, not for uv-specific reasons.
3. I'd expect the downstream user to specify the number version constraint if they want numpy 1 vs 2 instead? Can you say more about your use-case here?

---

_Comment by @Oddball777 on 2026-01-08 00:37_

My real life use case is mostly 1, just thought I'd ask about 2 at the same time since it seemed related and I was curious. Case 2 does fail when I try it.

For case 1, Poetry provides a way to make the separation between the base dependency and the CUDA-enabled variant explicit, for example:

```
dependencies = [
    "jax (>=0.4.36,<0.7) ; extra != 'cuda'",
]

[project.optional-dependencies]
cuda = [
    "jax[cuda] (>=0.4.36,<0.7)",
]
```

Is there an equivalent in uv or is it not necessary?

---
