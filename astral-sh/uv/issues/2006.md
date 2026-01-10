```yaml
number: 2006
title: "`PackageName` include non-normalized name"
type: issue
state: open
author: tdejager
labels:
  - compatibility
  - needs-decision
  - rustlib
assignees: []
created_at: 2024-02-27T08:35:43Z
updated_at: 2024-02-28T17:48:06Z
url: https://github.com/astral-sh/uv/issues/2006
synced_at: 2026-01-10T05:40:32Z
```

# `PackageName` include non-normalized name

---

_Issue opened by @tdejager on 2024-02-27 08:35_

Hi! We are currently integrating uv into pixi: https://github.com/prefix-dev/pixi/pull/863. Most of it works great, other than a few some things that could be improved. In RIP we also kept the source string as a base to refer back to, this is useful if people do not use a normalized name, and you want to work with the original name in some way.


See here:
https://github.com/prefix-dev/rip/blob/3d3590aac9722e219493f63f6c1fc5e9c5a91d7b/crates/rattler_installs_packages/src/types/package_name.rs

We would need something like this, we could keep the translation in our own source, but I could also add it to uv `PackageName`. Would you guys be interested in this? The trade-off is a bit more storage space for these things.

---

_Renamed from "PackageName include base name" to "`PackageName` include non-normalized name" by @tdejager on 2024-02-27 08:51_

---

_Comment by @konstin on 2024-02-27 10:27_

The main reason why we eventually chose not to include it is that different source can disagree about what the real name is, e.g. pypi says something different than the wheel metadata than a different index. Do you have a reliable source of display names? I'm also thinking about how we'd sync package name instances from different source.

---

_Comment by @tdejager on 2024-02-27 13:41_

Yeah so we didn't really worry about it, in the sense that we just wanted to know what the *original* name was. This is useful for keeping track of how it was specified in a manifest format.

I think the problem you are describing is a different one. If you don't feel like it makes sense within the uv context, then we can wrap it in a wrapper struct on our end :)

---

_Label `enhancement` added by @charliermarsh on 2024-02-27 16:11_

---

_Label `enhancement` removed by @charliermarsh on 2024-02-27 16:12_

---

_Label `compatibility` added by @charliermarsh on 2024-02-27 16:12_

---

_Label `needs-decision` added by @charliermarsh on 2024-02-27 16:12_

---

_Comment by @baszalmstra on 2024-02-28 10:46_

Similar to `VerbatimUrl` basically. We want to store the normalized package name as well as the original string it was created from.

---

_Label `rustlib` added by @zanieb on 2024-02-28 17:48_

---
