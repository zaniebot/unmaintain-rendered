```yaml
number: 14417
title: "`uv self update` for .exe from Github release / .msi installer"
type: issue
state: open
author: VaasuDevanS
labels:
  - enhancement
assignees: []
created_at: 2025-07-02T11:42:19Z
updated_at: 2025-07-02T13:17:39Z
url: https://github.com/astral-sh/uv/issues/14417
synced_at: 2026-01-12T16:01:48Z
```

# `uv self update` for .exe from Github release / .msi installer

---

_@VaasuDevanS_

### Summary

Thanks for making uv! I use it all the time for my personal projects, and I'd love to use it for work too.

Right now, when I try to install uv using the command, I get an error that says: `irm: The underlying connection was closed: An unexpected error occurred on a send`. I found out this happens because of my company's security rules.

I can get the newest version by downloading it from GitHub, but then I can't use the `uv self update` feature to easily update it later.

Would it be possible to add a way for the uv program to update itself even when I download it from GitHub? Or maybe you could offer an installer file (like an .msi file)? 

I also use [Pixi](https://pixi.sh/latest/installation/) from time to time and it provides both the options.

Thoughts?

---

_Label `enhancement` added by @VaasuDevanS on 2025-07-02 11:42_

---

_Comment by @zanieb on 2025-07-02 13:08_

We're generally interested in replacing cargo-dist with our own implementation which would support self update as you've described, but it's a not on the short-term roadmap.


---

_Comment by @VaasuDevanS on 2025-07-02 13:17_

Sounds good! It's definitely a valuable feature and will keep an eye out for it in the future

---
