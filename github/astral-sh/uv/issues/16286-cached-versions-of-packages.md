---
number: 16286
title: cached versions of packages
type: issue
state: open
author: MikeyBeez
labels:
  - enhancement
assignees: []
created_at: 2025-10-13T18:46:59Z
updated_at: 2025-10-13T19:34:58Z
url: https://github.com/astral-sh/uv/issues/16286
synced_at: 2026-01-07T13:12:19-06:00
---

# cached versions of packages

---

_Issue opened by @MikeyBeez on 2025-10-13 18:46_

### Summary

## How Your Idea Would Work
The workflow you're proposing would be a huge quality-of-life improvement. It might look something like this:
Your pyproject.toml has an unpinned dependency like "torch".
You run uv sync.
uv checks its global cache and sees that the last version of torch you installed from the nightly source was 2.7.0.dev20251012.
It then quickly checks the online repository and sees that a newer version, 2.7.0.dev20251013, is available.
Instead of just automatically downloading the new one, it prompts you:
Found a recent version of 'torch' (2.7.0.dev20251012) in your cache.
The latest version available is 2.7.0.dev20251013.

Use the cached version and skip the download? [Y/n]:
## Why It's a Great Suggestion
It Promotes Stability by Default: This makes your environment's versions "sticky." They won't change unless you explicitly tell them to, which is much safer than automatically grabbing the latest, potentially broken, nightly build.
It Saves Time and Bandwidth: It directly avoids the slow dependency resolution and re-download process for minor updates.
It's User-Centric: It makes the tool feel more like an intelligent assistant that understands your workflow, rather than just a command executor.

### Example

_No response_

---

_Label `enhancement` added by @MikeyBeez on 2025-10-13 18:47_

---

_Comment by @zanieb on 2025-10-13 19:34_

I don't think we'll add prompting for this, it doesn't really mesh with the overall user experience we're trying to build.

I think you are asking for a similar thing to https://github.com/astral-sh/uv/issues/16108 though.

> It Promotes Stability by Default: This makes your environment's versions "sticky." They won't change unless you explicitly tell them to, which is much safer than automatically grabbing the latest, potentially broken, nightly build.

This is provided by lockfiles. I recommend using them. If you're using `uv sync`, then you should be already. We don't fetch new versions unless you use `--refresh` or `--upgrade`.

---
