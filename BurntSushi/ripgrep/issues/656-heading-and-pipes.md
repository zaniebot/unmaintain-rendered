```yaml
number: 656
title: "--heading and pipes"
type: issue
state: closed
author: earnubs
labels: []
assignees: []
created_at: 2017-10-27T08:41:25Z
updated_at: 2017-11-07T16:14:40Z
url: https://github.com/BurntSushi/ripgrep/issues/656
synced_at: 2026-01-12T16:13:22Z
```

# --heading and pipes

---

_@earnubs_

Grouping (--heading, default tty) output is very useful, but it doesn't seem to work when piping:

```
rg -wtcss button | rg -ve '\.button' --heading
```

Is there a way to get the same grouping of results while also piping?

---

_Comment by @BurntSushi on 2017-10-27 11:15_

Unfortunately not. The latter invocation if ripgrep is just searching stdin, which it treats as a single file.

---

_Closed by @earnubs on 2017-11-07 16:14_

---
