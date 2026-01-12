```yaml
number: 3112
title: "globset: allow nested alternatives"
type: pull_request
state: closed
author: senekor
labels:
  - rollup
assignees: []
base: master
head: senekor/mqozuyzrvksv
created_at: 2025-07-28T19:33:27Z
updated_at: 2025-11-15T15:26:03Z
url: https://github.com/BurntSushi/ripgrep/pull/3112
synced_at: 2026-01-12T18:23:15Z
```

# globset: allow nested alternatives

---

_@senekor_

Hello. I have a use case where it would be great if nested alternatives were supported in globset. Namely, the [Helix editor](github.com/helix-editor/helix) uses globset for [Editorconfig](https://spec.editorconfig.org/), which allows nested alternatives. It is used in the real world by the Linux kernel (see the [Linux editorconfig](https://github.com/torvalds/linux/blob/master/.editorconfig#L5) with nested alternatives).

Unix shells like Bash and Fish support this kind of syntax and the documentation of globset states that "Nesting `{...}` is not **currently** allowed" (emphasis mine)

So I assume this feature is in-scope for the globset crate. I'm happy to improve the implementation if there is any feedback. Thank you for taking a look!

---

_Comment by @BurntSushi on 2025-07-28 19:44_

Looks like a duplicate of #3048? (I haven't yet looked at that PR or this one, so I don't know the differences between them.)

---

_Comment by @senekor on 2025-07-28 19:47_

Oh yeah, that seems to be the case. Apologies, I only searched issues and not PRs before posting.

---

_Label `rollup` added by @BurntSushi on 2025-08-17 13:22_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---

_Branch deleted on 2025-11-15 15:26_

---
