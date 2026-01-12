```yaml
number: 16017
title: rocm7.0 and rocm7.9 should be supported as torch backends, you might also need rocm7.0+gfx110X as a label
type: issue
state: open
author: doctorpangloss
labels:
  - bug
assignees: []
created_at: 2025-09-24T17:00:13Z
updated_at: 2025-09-24T17:00:13Z
url: https://github.com/astral-sh/uv/issues/16017
synced_at: 2026-01-12T16:02:21Z
```

# rocm7.0 and rocm7.9 should be supported as torch backends, you might also need rocm7.0+gfx110X as a label

---

_@doctorpangloss_

### Summary

The ROCm situation is kind of complicated

There are flat indices that are more supported than the pytorch ones, all here https://rocm.nightlies.amd.com/v2 - for example with support for all gfx1100, gfx1101, gfx1102 GPUs: https://rocm.nightlies.amd.com/v2/gfx110X-dgpu/torch/ . pytorch.org binaries don't have all the GPU backends built in

you can see this allows alignment of the rocm, torch and container runtimes - https://hub.docker.com/r/rocm/pytorch/tags?name=rocm7.0

NVIDIA chooses to bundle all its GPU architectures with +cu128 builds, so +cu128_sm8x isn't something that appears in the wild. But everything else implicitly does bundle a limited number of GPU architectures, such as SageAttention and flash_attn.

I understand from a product point of view you are authoring a JFrog / Artifactory thing as a solution to this problem. Of course, it is just string comparisons in 90% of cases, which you could do on the client, and in principle, for the 10% of cases, all the metadata that your backend is going to be storing from extracted wheels could be distributed with `uv` in a sqlite file, it is probably less than 10MB of static compressed text content. It is your prerogative.

### Platform

Ubuntu

### Version

0.8.22

### Python version

Python 3.12.6

---

_Label `bug` added by @doctorpangloss on 2025-09-24 17:00_

---
