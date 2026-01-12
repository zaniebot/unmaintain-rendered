```yaml
number: 1689
title: "[FR] Have `--read0` that reads the input records separated by null"
type: issue
state: closed
author: NightMachinery
labels: []
assignees: []
created_at: 2020-09-26T13:28:47Z
updated_at: 2020-09-26T14:00:34Z
url: https://github.com/BurntSushi/ripgrep/issues/1689
synced_at: 2026-01-12T16:13:24Z
```

# [FR] Have `--read0` that reads the input records separated by null

---

_@NightMachinery_

Sometimes, the records we want to filter have newlines in them, so we can't use the newline as input record separator. Having `--read0` would solve this problem:

```
echo 123$'\0'456$'\0'789 | rg --read0 56
# => 456
```


---

_Comment by @BurntSushi on 2020-09-26 13:41_

Why doesn't `--null-data` work for you?

---

_Comment by @NightMachinery on 2020-09-26 13:51_

[**@BurntSushi**](https://github.com/BurntSushi) commented on [Sep 26, 2020, 5:11 PM GMT+3:30](https://github.com/BurntSushi/ripgrep/issues/1689#issuecomment-699497163 "2020-09-26T13:41:14Z - Replied by Github Reply Comments"):
> Why doesn't `--null-data` work for you?

Sorry, I mistakenly read that as being equivalent to `--print0`. Thanks!

---

_Closed by @NightMachinery on 2020-09-26 13:51_

---

_Comment by @BurntSushi on 2020-09-26 14:00_

Aye, and note that `--null-data` can also be found in GNU grep.

---
