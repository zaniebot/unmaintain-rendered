---
number: 12258
title: Consider supporting non-python packages and tools for dev toolchains
type: issue
state: open
author: strangemonad
labels:
  - enhancement
assignees: []
created_at: 2025-03-18T02:14:36Z
updated_at: 2025-03-18T02:14:36Z
url: https://github.com/astral-sh/uv/issues/12258
synced_at: 2026-01-07T13:12:18-06:00
---

# Consider supporting non-python packages and tools for dev toolchains

---

_Issue opened by @strangemonad on 2025-03-18 02:14_

### Summary

After a quick search, I wasn't able to find any similar request. I haven't thought very deeply about how this would all work but at a high-level, there's often the requirement to have other tool dependencies in development (I'm considering runtime dependencies like native shared libraries separately).

I could imagine a this taking a couple different forms:
- a flavor of `uv tool ...` that's a wrapper around some other tool runner eg `npx`. Some examples include the supabase cli (npx) for local development and ci/cd. I could also imagine things like the work needed for the pyright wrapper https://pypi.org/project/pyright/ wouldn't be needed for such tools.
- Sometimes you just need native deps eg in the bio world, you find yourself installing things like samtools, bowtie etc because their python wrappers just expect the binary on your path. Bio-conda is one approach to solving the bio informatics toolchain setup. When I also need those on the deployment environment, I currently make sure they're installed in the deployed docker image.

There's obviously prior art in this space (coda, pixie, nix dev-envs/determinant-systems, ...).

### Example

_No response_

---

_Label `enhancement` added by @strangemonad on 2025-03-18 02:14_

---
