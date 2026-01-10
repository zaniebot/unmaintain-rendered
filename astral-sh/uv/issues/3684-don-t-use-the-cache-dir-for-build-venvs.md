---
number: 3684
title: "Don't use the cache dir for build venvs?"
type: issue
state: open
author: thejcannon
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2024-05-21T01:22:04Z
updated_at: 2024-05-21T11:56:35Z
url: https://github.com/astral-sh/uv/issues/3684
synced_at: 2026-01-10T01:23:30Z
---

# Don't use the cache dir for build venvs?

---

_Issue opened by @thejcannon on 2024-05-21 01:22_

👋 We're hoping to network mount a read-only (hot) cache for `uv` so our servers can bootstrap themselves quickly.

Unfortunately since the tempdir for build isolation is installed under the cache dir, there's no good way to do this.

I think the issue is in:

https://github.com/astral-sh/uv/blob/03629181962e1f358361679d8b30bf48f386ad1d/crates/uv-build/src/lib.rs#L404

(Out of curiosity, why not put it in the default tempdir location per-platform?)

---

_Comment by @charliermarsh on 2024-05-21 01:30_

You can't hardlink or reflink across filesystems, and often the default tempdir is on a different drive or partition than wherever you put the cache. Using the cache for the temporary build environment ensures they're colocated.

---

_Comment by @thejcannon on 2024-05-21 01:35_

That in itself makes sense, I'm guessing the benefit there is along the lines of "we can fast-install the build requirements from the cache because we know they're colocated?"

---

_Comment by @thejcannon on 2024-05-21 01:37_

Would you be open to controlling the tempdirs via a flag? And it defaults to the cache dir?

---

_Comment by @charliermarsh on 2024-05-21 11:56_

I'm open to it... We'd need to figure out what to call it and what it "means". Can you look around at all the `tempfile` uses that would be affected by this decision? We tend to always use the cache dir for this.

---

_Label `configuration` added by @charliermarsh on 2024-05-21 11:56_

---

_Label `needs-decision` added by @charliermarsh on 2024-05-21 11:56_

---

_Referenced in [astral-sh/uv#12036](../../astral-sh/uv/issues/12036.md) on 2025-03-07 09:35_

---

_Referenced in [astral-sh/uv#13096](../../astral-sh/uv/issues/13096.md) on 2025-04-25 20:37_

---
