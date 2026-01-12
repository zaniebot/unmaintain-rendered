```yaml
number: 3014
title: "ignore: publicize ignore::dir"
type: issue
state: closed
author: noahbkim
labels:
  - wontfix
assignees: []
created_at: 2025-03-07T16:44:33Z
updated_at: 2025-09-23T01:18:41Z
url: https://github.com/BurntSushi/ripgrep/issues/3014
synced_at: 2026-01-12T16:13:25Z
```

# ignore: publicize ignore::dir

---

_@noahbkim_

I was wondering if any of the maintainers had a philosophical objection to making `ignore::dir::Ignore` and related machinery part of the public API. I've found myself needing to reimplement `Walk` with slightly modified file system abstractions, and it would be really helpful to be able to reuse the `Ignore` bits.

Happy to put up the PR if this seems reasonable.

---

_Comment by @BayLee4 on 2025-03-20 18:42_

This would be very useful especially with the ability to clone `WalkBuilder.ig_builder`, that would allow to check if a currently non-existing file/directory would be ignored before creating it.

---

_Comment by @BurntSushi on 2025-09-23 01:18_

I'm unwilling to do this at this time. The `ignore` crate needs to be re-designed, and opening up more of its internals is really not something I am willing to support.

---

_Closed by @BurntSushi on 2025-09-23 01:18_

---

_Label `wontfix` added by @BurntSushi on 2025-09-23 01:18_

---
