```yaml
number: 1885
title: "Add `*.ru` to ruby type (and maybe more)"
type: issue
state: closed
author: BuonOmo
labels: []
assignees: []
created_at: 2021-06-01T08:16:09Z
updated_at: 2021-06-01T10:20:28Z
url: https://github.com/BurntSushi/ripgrep/issues/1885
synced_at: 2026-01-12T16:13:24Z
```

# Add `*.ru` to ruby type (and maybe more)

---

_@BuonOmo_

Thanks for the tool I'm using the most!

As a ruby dev, it feels like the `type` ruby is a bit incomplete.

https://github.com/BurntSushi/ripgrep/blob/df83b8b44426b3f2179abe632eb183e8c8270524/crates/ignore/src/default_types.rs#L176

I suggest adding `*.ru` and maybe `*.erb`. Since the first one is `rack` specific (most web apps) it feels like a nobrainer. The second one however already has its dedicater type and is not the only templating engine for ruby, hence it could be added to a more global `rails` type?

---

_Comment by @BurntSushi on 2021-06-01 10:20_

Adding default file types is such a small change that I ask that you just open a PR. It sounds like `ru` is a good candidate to go into the existing `ruby` type.

If it's not clear where `erb` should go, then it's definitely not clear to me. :-) I suggest asking other Ruby devs what should be done.

---

_Closed by @BurntSushi on 2021-06-01 10:20_

---
