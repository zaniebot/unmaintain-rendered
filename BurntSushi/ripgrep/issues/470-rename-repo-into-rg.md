```yaml
number: 470
title: rename repo into rg?
type: issue
state: closed
author: kindlychung
labels: []
assignees: []
created_at: 2017-05-01T11:33:34Z
updated_at: 2017-05-01T12:11:39Z
url: https://github.com/BurntSushi/ripgrep/issues/470
synced_at: 2026-01-12T16:13:22Z
```

# rename repo into rg?

---

_@kindlychung_

It's counter-intuitive that we need to `cargo install ripgrep` only to use the `rg` executable. 

---

_Comment by @BurntSushi on 2017-05-01 11:40_

Sorry, but no.

If it were up to me, I would have called the executable ripgrep. But I would have been overrun by complaints that the executable name was too long. Therefore, I wanted it to be short. But if it's short, then it's hard for it to carry any significant meaning and makes it harder to search for. ripgrep is a meatier name, and by using it in as many places as I can (*except* for the executable name), it makes it much more discoverable.

The situation is lamentable, and I think your particular issue is, at best, a very minor nuisance compared to the gains.

---

_Closed by @BurntSushi on 2017-05-01 11:41_

---

_Comment by @BurntSushi on 2017-05-01 11:43_

Also, a minor note: the title of this issue asks to rename the repository, but the content if your issue mentions `cargo install ripgrep`. The repository name on github is distinct from the crate name on crates.io. "ripgrep" is also used in various packaging repositories, such as brew and Archlinux. It's not changing.

---
