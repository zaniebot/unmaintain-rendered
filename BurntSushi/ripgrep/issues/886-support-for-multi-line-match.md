```yaml
number: 886
title: Support for multi-line match
type: issue
state: closed
author: kenorb
labels:
  - duplicate
assignees: []
created_at: 2018-04-15T00:48:53Z
updated_at: 2018-04-15T00:50:09Z
url: https://github.com/BurntSushi/ripgrep/issues/886
synced_at: 2026-01-12T16:13:22Z
```

# Support for multi-line match

---

_@kenorb_

#### What version of ripgrep are you using?

`ripgrep 0.8.1`

#### What operating system are you using ripgrep on?

macOS High Sierra

#### Feature

I am running the following command:

    printf "foo\r\nbar\r\n" | rg "(?ms)foo.*bar"

and I would expect the match given the [multi-line mode](https://github.com/robinst/regex/blob/master/src/lib.rs#L341).

---

_Comment by @BurntSushi on 2018-04-15 00:50_

@kenorb Filing a flurry of issues without searching the issue tracker isn't appreciated. 

Dupe of #176.

---

_Closed by @BurntSushi on 2018-04-15 00:50_

---

_Label `duplicate` added by @BurntSushi on 2018-04-15 00:50_

---
