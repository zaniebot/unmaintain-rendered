```yaml
number: 2292
title: short arg for --files-without-match
type: issue
state: closed
author: jmhodges-color
labels:
  - wontfix
assignees: []
created_at: 2022-08-26T20:07:42Z
updated_at: 2023-11-24T19:18:52Z
url: https://github.com/BurntSushi/ripgrep/issues/2292
synced_at: 2026-01-12T16:13:24Z
```

# short arg for --files-without-match

---

_@jmhodges-color_

#### Describe your feature request

It would be nice if there was a shorter version of the `--files-without-match` argument to ripgrep. While `ripgrep` can't use `-L` (like `grep`, `ag`, and `ack`) because it's taken for `--follow`, it would be nice to have another one.

What do you think about `-K` for that? Or `-W`?

(This is with ripgrep 13.0.0 on macOS 12.5.1 installed via homebrew)


---

_Comment by @BurntSushi on 2022-08-26 21:13_

Yes, I used `-L` for `--follow` because I suspect it is more common.

I'm not inclined to add a short flag here personally. I think the flag is probably used rarely enough that it doesn't warrant one. And to be honest, neither `-K` or `-W` strike me as particularly good options. `-L` would be the best choice, and if it were available, I would consider adding it as a short flag for `--files-without-match`.

---

_Label `enhancement` added by @BurntSushi on 2022-08-26 21:13_

---

_Label `question` added by @BurntSushi on 2022-08-26 21:13_

---

_Comment by @BurntSushi on 2023-11-24 19:18_

I think what I said before still stands today and I'm not inclined to think differently.

I do wish I could go back in time and be a little more careful about the short flags I used. But that ship has sailed.

---

_Closed by @BurntSushi on 2023-11-24 19:18_

---

_Label `enhancement` removed by @BurntSushi on 2023-11-24 19:18_

---

_Label `question` removed by @BurntSushi on 2023-11-24 19:18_

---

_Label `wontfix` added by @BurntSushi on 2023-11-24 19:18_

---
