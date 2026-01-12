```yaml
number: 1328
title: Update deps
type: pull_request
state: closed
author: tiziano88
labels: []
assignees: []
base: master
head: tzn_deps
created_at: 2019-07-25T10:13:09Z
updated_at: 2019-07-25T11:38:44Z
url: https://github.com/BurntSushi/ripgrep/pull/1328
synced_at: 2026-01-12T18:23:13Z
```

# Update deps

---

_@tiziano88_

- Relax `crossbeam-channel` constraint slightly
- Run `cargo update`.

---

_Review comment by @BurntSushi on `ignore/Cargo.toml`:21 on 2019-07-25 10:44_

Why are you relaxing this version constraint? Have you tested that `ignore` works with `crossbeam-channel` `0.3.0` through `0.3.5`?

---

_@BurntSushi requested changes on 2019-07-25 10:45_

Sorry, but what problem are you trying to solve here? I usually do this myself, because I use it as an opportunity to inspect what dependencies have been updated (or any new dependencies that have been introduced) and then audit them, if necessary.

---

_@tiziano88 reviewed on 2019-07-25 11:15_

---

_Review comment by @tiziano88 on `ignore/Cargo.toml`:21 on 2019-07-25 11:15_

The reason is that I have a patched (for unrelated reasons) version of `crossbeam-channel` `0.3.9` already in my deps tree, but `ignore` is still depending on `0.3.6.` and therefore not picking up my local patch.

To your question, no, I have not actually checked that it works with these versions, I am just assuming that minor version increments would be compatible. If you want to do this more carefully, feel free to close this PR and do it in your own time, I agree it makes sense to inspect dependencies for a binary like `rg` that gets installed by so many people.

Thanks, and keep up the good work!

---

_Review comment by @BurntSushi on `ignore/Cargo.toml`:21 on 2019-07-25 11:38_

`0.3.6` is equivalent to `^0.3.6`, which is equivalent to `>= 0.3.6, < 0.4.0`, so there should be no problem with Cargo using `0.3.9` with `ignore` due to this specific constraint. Something else must be going on?

But yeah, all right, I'm going to close this. I'll see about doing a dependency update soonish.

---

_@BurntSushi reviewed on 2019-07-25 11:38_

---

_Closed by @BurntSushi on 2019-07-25 11:38_

---
