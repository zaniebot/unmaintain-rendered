---
number: 11563
title: UV-pack?
type: issue
state: open
author: dipta007
labels:
  - enhancement
assignees: []
created_at: 2025-02-16T23:28:23Z
updated_at: 2025-08-07T19:19:09Z
url: https://github.com/astral-sh/uv/issues/11563
synced_at: 2026-01-07T13:12:18-06:00
---

# UV-pack?

---

_Issue opened by @dipta007 on 2025-02-16 23:28_

### Summary

Can we have something like uv-pack similar to [conda-pack](https://conda.github.io/conda-pack/)?
It will really help in reproducibility and sharing environment in the ML community, where the libs are too fast evolving and need kernel dependency.

### Example

- To pack the locak venv - `uv-pack`. It will pack into `venv.tar.gz`
- Copy the tar to another environment
- `uv-unpack venv.tar.gz`
- Voila!! the env is reproduced

---

_Label `enhancement` added by @dipta007 on 2025-02-16 23:28_

---

_Comment by @dipta007 on 2025-02-16 23:53_

After some more google & perplexity: I found this: [venv-pack](https://jcristharif.com/venv-pack/)

---

_Comment by @timvink on 2025-02-17 09:20_

Another approach would be to have `uv build --wheel --pinned` (https://github.com/astral-sh/uv/issues/8729), then you could just share the wheel file (much smaller), as `uv` is super fast at installing python+dependencies anyway.

---

_Comment by @dipta007 on 2025-02-17 17:30_

> Another approach would be to have `uv build --wheel --pinned` ([#8729](https://github.com/astral-sh/uv/issues/8729)), then you could just share the wheel file (much smaller), as `uv` is super fast at installing python+dependencies anyway.

@timvink Wow didn't know about wheel. Can you give a short example on what to do one Machine A (host) and on Machine B (new env) to get the same env? Really thanks.

---

_Comment by @lminer on 2025-08-07 19:19_

I second that this would be useful

---
