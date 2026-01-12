```yaml
number: 489
title: Update documentation for --color ansi
type: pull_request
state: merged
author: ericbn
labels: []
assignees: []
merged: true
base: master
head: man
created_at: 2017-05-25T22:08:24Z
updated_at: 2017-05-25T23:02:24Z
url: https://github.com/BurntSushi/ripgrep/pull/489
synced_at: 2026-01-12T18:23:13Z
```

# Update documentation for --color ansi

---

_@ericbn_

In `src/app.rs`, change typo "When ansi used" to "When ansi is used".
Update man pages with missing `ansi` option for `--color`.

---

_@BurntSushi approved on 2017-05-25 22:09_

Nice! Thank you!

---

_Comment by @ericbn on 2017-05-25 22:13_

One not-so-closely-related question: why isn't the `fn color(&self) -> bool` in `src/args.rs` testing for the "ansi" value? As fair as I got it, it would return false if preference == "ansi"...



---

_Comment by @BurntSushi on 2017-05-25 22:27_

Looks like that function is vestigial. If you look below it, there is a `color_choice` function that appears to be used now.

---

_Comment by @ericbn on 2017-05-25 22:35_

Yeah, it's not being used anymore. Created #490

---

_Merged by @BurntSushi on 2017-05-25 22:58_

---

_Closed by @BurntSushi on 2017-05-25 22:58_

---

_Branch deleted on 2017-05-25 23:02_

---
