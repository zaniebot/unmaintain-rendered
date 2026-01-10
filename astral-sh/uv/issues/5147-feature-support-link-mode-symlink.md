---
number: 5147
title: "feature: support `--link-mode=symlink`"
type: issue
state: closed
author: hutch3232
labels:
  - enhancement
  - good first issue
  - help wanted
assignees: []
created_at: 2024-07-17T15:43:52Z
updated_at: 2025-09-19T19:33:20Z
url: https://github.com/astral-sh/uv/issues/5147
synced_at: 2026-01-10T01:23:45Z
---

# feature: support `--link-mode=symlink`

---

_Issue opened by @hutch3232 on 2024-07-17 15:43_

Currently `--link-mode` supports `copy`, `hardlink`, and `clone`. I have my `UV_CACHE_DIR` set to a mounted drive which prevents hardlinks and cloning ("invalid cross-device link"). Symlinks would work, however. This could really increase performance for my situation, rather than copying over a bunch of files. When I work in R, I use `renv` which uses symlinks to a central cache successfully in the proposed way: https://github.com/rstudio/renv.

More details about the specific use-case:
I work in ephemeral docker containers. My workflow is generally to start by running `uv pip sync requirements.txt` which currently is triggering a copy for each package. I mount a persistent drive to my container so that my `uv` cache persists from session to session. Also worth noting that some collaborators mount the same drive and use the same cache, so that we benefit from each others' install time. Hopefully that isn't too risky, but it has worked well so far and works great in the aforementioned `renv` package in R.

Related issue:
https://github.com/astral-sh/uv/issues/2103

Thanks for making this amazing project!

---

_Comment by @charliermarsh on 2024-07-17 15:44_

I don't mind supporting this, I was kinda waiting for it to be requested.

---

_Label `enhancement` added by @charliermarsh on 2024-07-17 16:05_

---

_Label `good first issue` added by @charliermarsh on 2024-07-17 16:05_

---

_Label `help wanted` added by @charliermarsh on 2024-07-17 16:05_

---

_Comment by @charliermarsh on 2024-07-17 16:06_

Should be straightforward-ish if anyone wants to submit a PR -- it'd basically be copying the hardlink code, but changing the operation we use at the end.

---

_Comment by @danielenricocahall on 2024-07-19 01:20_

Hi @charliermarsh! Would it be a matter of swapping `fs::hard_link` for `std::os::unix::fs::symlink` (after adding a `Symlink` value to the enum and mapping it to a new callable?

---

_Comment by @charliermarsh on 2024-07-19 01:26_

@danielenricocahall - Yeah that sounds roughly correct to me.

---

_Comment by @charliermarsh on 2024-07-19 01:27_

We'll also need `std::os::windows::fs::symlink_file` or similar on Windows which will probably fail for most people due to how symlink permissions work on Windows, but that's fine, they just shouldn't opt-in to this mode if they haven't enabled symlinks :)

---

_Referenced in [astral-sh/uv#5208](../../astral-sh/uv/pulls/5208.md) on 2024-07-19 01:41_

---

_Comment by @charliermarsh on 2024-07-19 12:41_

Closed by https://github.com/astral-sh/uv/pull/5208. Thanks @danielenricocahall!

---

_Closed by @charliermarsh on 2024-07-19 12:41_

---

_Comment by @helderco on 2024-07-20 02:34_

I was about to ask for this for the same reasons and just found out it’s been released already! 🎉

---

_Comment by @skshetry on 2024-07-20 05:05_

I wonder if `uv` can symlink packages directly instead of per-file (similar to `virtualenv --symlink-app-data`), which could make it much faster than hardlink/clone.
 
On macOS, you can also clone a directory, but I see that `uv` already does just that.




---

_Comment by @charliermarsh on 2024-07-20 11:06_

That’s a good idea. We should probably symlink at the directory level.

---

_Referenced in [astral-sh/uv#5244](../../astral-sh/uv/issues/5244.md) on 2024-07-20 11:15_

---

_Comment by @charliermarsh on 2024-07-20 11:15_

https://github.com/astral-sh/uv/issues/5244

---

_Comment by @frainfreeze on 2025-09-19 19:24_

are there docs for this?

---

_Comment by @hutch3232 on 2025-09-19 19:33_

https://docs.astral.sh/uv/reference/settings/#link-mode

The docs mention and I'll emphasize that the devs (with good reasons) discourage using symlinks.

Anecdotally they've worked tremendously well for me. I've been using them every work day since this feature was released.

---
