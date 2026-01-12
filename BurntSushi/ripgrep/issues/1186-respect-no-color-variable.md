```yaml
number: 1186
title: Respect $NO_COLOR variable
type: issue
state: closed
author: Sohalt
labels:
  - enhancement
  - question
assignees: []
created_at: 2019-02-04T19:06:15Z
updated_at: 2020-01-11T15:10:25Z
url: https://github.com/BurntSushi/ripgrep/issues/1186
synced_at: 2026-01-12T16:13:23Z
```

# Respect $NO_COLOR variable

---

_@Sohalt_

It would be cool for ripgrep to respect the NO_COLOR "standard", i.e. when $NO_COLOR is set, ripgrep should not colorize the output. See https://no-color.org

---

_Comment by @BurntSushi on 2019-02-04 19:17_

I think I'm OK with this in principle. It should be implemented in [`termcolor`](https://github.com/BurntSushi/termcolor) as parts of its ["auto"](https://docs.rs/termcolor/1.0.4/termcolor/enum.ColorChoice.html#variant.Auto) mode.

I think I've historically not done this because it's not clear to me how widespread `NO_COLOR` usage actually is.

---

_Label `enhancement` added by @BurntSushi on 2019-02-04 19:17_

---

_Label `question` added by @BurntSushi on 2019-02-04 19:17_

---

_Comment by @Sohalt on 2019-02-04 21:43_

Thanks, and sorry for not doing my research. I suspected ripgrep to use some library for colored terminal output and that would obviously be the better place to implement this. I've opened https://github.com/BurntSushi/termcolor/issues/13.
As for how widespread `NO_COLOR` usage is, I can't really say, but I don't see a big downside to not implementing it. The variable name is fairly specific and even if it is set, one can override it, i.e. turn on colors, on a per program basis.
You can probably close this issue, since it needs to be implemented in termcolor and the change will eventually show up in ripgrep, when dependencies are updated.

---

_Comment by @BurntSushi on 2019-02-05 11:58_

Please take further discussion to https://github.com/BurntSushi/termcolor/issues/13

---

_Closed by @BurntSushi on 2020-01-11 15:10_

---
