```yaml
number: 2165
title: Exclude Directories Not Working
type: issue
state: closed
author: mattmccormick
labels:
  - duplicate
assignees: []
created_at: 2022-03-18T18:12:30Z
updated_at: 2022-03-18T18:18:05Z
url: https://github.com/BurntSushi/ripgrep/issues/2165
synced_at: 2026-01-12T16:13:24Z
```

# Exclude Directories Not Working

---

_@mattmccormick_

Hi there,

I'm trying to exclude directories from my search.  I'm using the following command:

```shell
rg --fixed-strings --max-filesize 50K --max-count 1 --glob '![/sys/devices/*, /proc/*]' --files espanso /
```

But in the output I clearly see files that match directories starting with `/proc`.  What am I doing wrong?  I've also tried a bunch of other things like using double stars.

I'm using version 11.0.2

---

_Comment by @BurntSushi on 2022-03-18 18:17_

This looks like a duplicate of #2156.

---

_Label `duplicate` added by @BurntSushi on 2022-03-18 18:17_

---

_Closed by @BurntSushi on 2022-03-18 18:17_

---

_Comment by @BurntSushi on 2022-03-18 18:18_

(Also, I don't know where the `[x, y]` glob syntax came from. That's not standard at all and is certainly not supported by ripgrep.)

---
