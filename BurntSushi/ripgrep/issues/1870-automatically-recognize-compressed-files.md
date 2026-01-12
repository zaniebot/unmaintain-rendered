```yaml
number: 1870
title: Automatically recognize compressed files
type: issue
state: closed
author: smprather
labels:
  - invalid
assignees: []
created_at: 2021-05-25T20:33:15Z
updated_at: 2021-05-26T10:35:01Z
url: https://github.com/BurntSushi/ripgrep/issues/1870
synced_at: 2026-01-12T16:13:24Z
```

# Automatically recognize compressed files

---

_@smprather_

I regularly scan directories recursively. Some of those files may be gzipped, some not. Not to mention it is a minor pain in the neck to remember to add -z if grepping a compressed file. I can't just write a wrapper because I rely on rg's directory recursion a lot. Sure, I could wrap for that too, but then it starts getting hairy.

It would be really helpful if rg could automatically recognize compressed files and do The Right Thing. Initial support could just look at the file extension and not attempt recognition by internal file scanning. Thanks! 

---

_Comment by @BurntSushi on 2021-05-25 20:53_

Sorry, but what is the problem with `alias rg="rg -z"`?

---

_Comment by @smprather on 2021-05-26 08:36_

Nothing wrong with that, except that I didn't know the flag would still search uncompressed files :). I though -z was used specifically for compressed file searches, but not for others. Thanks @BurntSushi .

---

_Closed by @smprather on 2021-05-26 08:36_

---

_Label `invalid` added by @BurntSushi on 2021-05-26 10:35_

---
