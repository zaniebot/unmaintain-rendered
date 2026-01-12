```yaml
number: 853
title: Port grep to new regex-syntax
type: issue
state: closed
author: igor-raits
labels:
  - enhancement
assignees: []
created_at: 2018-03-12T10:15:04Z
updated_at: 2018-03-14T02:55:41Z
url: https://github.com/BurntSushi/ripgrep/issues/853
synced_at: 2026-01-12T16:13:22Z
```

# Port grep to new regex-syntax

---

_@igor-raits_

https://github.com/BurntSushi/ripgrep/blob/00520b30f5f38e543e17b1a4cc5e8417bc488ea4/grep/Cargo.toml#L19

We can't update regex-syntax in Fedora to 0.5 because of grep holding 0.4. Would be nice if you could fix it. Thanks!

---

_Comment by @BurntSushi on 2018-03-12 10:48_

Umm. It's on my list of things to do, but I don't understand why this is blocking an update. It sounds like a pretty serious issue, because it means you're blocked on a single programmer's capacity to migrate crates. This isn't just a trivial update either (as intended).

Also, the current release of ripgrep (according to `Cargo.lock`) should only depend on a single version of `regex-syntax`.

---

_Comment by @igor-raits on 2018-03-12 10:55_

@BurntSushi we try to keep only latest version of crates. So we could add second (0.4) and update primary to 0.5, but we try to not do this as hard as possible ;)

---

_Comment by @BurntSushi on 2018-03-12 10:56_

All righty. I mean, I definitely want to move to the new regex-syntax as quickly as possible since it fixes a few bugs and also lets ripgrep use the new regex features. So it's a priority, at least. :-)

---

_Label `enhancement` added by @BurntSushi on 2018-03-13 02:19_

---

_Assigned to @BurntSushi by @BurntSushi on 2018-03-13 02:19_

---

_Closed by @BurntSushi on 2018-03-14 02:55_

---
