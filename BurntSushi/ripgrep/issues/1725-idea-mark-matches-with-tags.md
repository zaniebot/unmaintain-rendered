```yaml
number: 1725
title: "idea: mark matches with tags"
type: issue
state: closed
author: 0x7FFFFFFFFFFFFFFF
labels:
  - question
assignees: []
created_at: 2020-11-03T19:41:12Z
updated_at: 2020-11-03T20:08:11Z
url: https://github.com/BurntSushi/ripgrep/issues/1725
synced_at: 2026-01-12T16:13:24Z
```

# idea: mark matches with tags

---

_@0x7FFFFFFFFFFFFFFF_

#### Describe your feature request

Right now ripgrep has a `--colors=COLORS, --colours=COLORS` parameter to let the user mark different parts of the matches with different colors. However, this is only useful in a console environment. When the result of ripgrep was redirected to a file, all colors will disappear. If there are options like `--tag-start` and `--tag-end` to let the user mark the matches, it will improve the usability of the program. If we have `--tag-start` and `--tag-end` options, we can use `rg --tag-start "<div>" --tag-end "</div>" example test.txt` to get results like `<div>example</div>`.

---

_Comment by @BurntSushi on 2020-11-03 20:00_

Use the `-r/--replace` flag.

---

_Closed by @BurntSushi on 2020-11-03 20:00_

---

_Label `question` added by @BurntSushi on 2020-11-03 20:00_

---

_Comment by @0x7FFFFFFFFFFFFFFF on 2020-11-03 20:08_

Thanks! I missed this option and it work well.

---
