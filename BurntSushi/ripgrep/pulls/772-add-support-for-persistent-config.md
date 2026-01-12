```yaml
number: 772
title: add support for persistent config
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/persistent-config
created_at: 2018-02-04T05:10:43Z
updated_at: 2018-02-14T18:08:11Z
url: https://github.com/BurntSushi/ripgrep/pull/772
synced_at: 2026-01-12T18:23:13Z
```

# add support for persistent config

---

_@BurntSushi_

This PR fixes both #196 and #553. The commit messages have most of the details (including some goodies like a 2x decrease in release build time), but the TL;DR is that ripgrep now recognizes a `RIPGREP_CONFIG_PATH` environment variable. When set and non-empty, ripgrep will read shell arguments from it (one per line) and prepend them to whatever explicit CLI arguments have been given. A new flag, `--no-config`, prevents any sort of configuration from being loaded from a file.

This does not add any support for auto-loading config files from pre-determined locations like one's `$XDG_CONFIG_HOME` directory. [It was decided](https://github.com/BurntSushi/ripgrep/issues/196#issuecomment-362850321) to punt on that for now, and one wonders how far we can get with just the env var approach!

---

_Comment by @okdana on 2018-02-04 05:17_

To fix the completion-function check you can add this to `_rg` (around line 55):

```
"--no-config[don't read configuration files]"
```

---

_Comment by @BurntSushi on 2018-02-04 05:22_

@okdana Yup! Figured it out. :-) I'm quite happy that CI caught that! I completely forgot we even had the test.

---

_Merged by @BurntSushi on 2018-02-04 15:40_

---

_Closed by @BurntSushi on 2018-02-04 15:40_

---

_Branch deleted on 2018-02-04 15:40_

---

_Comment by @hoodie on 2018-02-12 09:02_

this is a neat feature, but it falls down if you install from crates.io:
```
ripgrep 0.8.0 (rev )
```

---

_Comment by @BurntSushi on 2018-02-12 12:19_

@hoodie Thanks, I created #789. It is low priority though.

---

_Comment by @BatmanAoD on 2018-02-14 18:08_

@BurntSushi Just wanted to say that 0.8 is a great update; I just migrated to Windows 10 so it's great to have native colors in `cmd`, and the persistent config is very nice. Thanks for the awesome tool!

---
