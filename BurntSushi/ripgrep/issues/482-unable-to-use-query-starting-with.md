```yaml
number: 482
title: "Unable to use query starting with \"-\""
type: issue
state: closed
author: RalfJung
labels:
  - bug
assignees: []
created_at: 2017-05-15T16:52:08Z
updated_at: 2017-07-30T22:05:01Z
url: https://github.com/BurntSushi/ripgrep/issues/482
synced_at: 2026-01-12T16:13:22Z
```

# Unable to use query starting with "-"

---

_@RalfJung_

Calling `rg -e --` (to search for all occurrences of "--") fails with
```
$ rg -e "--"
error: The argument '--regexp <pattern>...' requires a value but none was supplied
```
In contrast, the same pattern works for the usual GNU-style tools:
```
$  egrep -e "--" -R .
...
```

I am aware that I can work around this issue using e.g. `[-]-` as the pattern, but that should not be necessary.

---

_Comment by @BurntSushi on 2017-05-15 17:20_

This might be a clap bug. Note that `rg -e -` works.

cc @kbknapp 

---

_Comment by @kbknapp on 2017-05-15 17:33_

It looks like it's due to the special casing of `--` meaning "only free args follow" taking precedence over the `-e` allowing values to start with `-`. This should be a decently easy fix, I think I can knock it out later today if all goes well.

---

_Comment by @BurntSushi on 2017-05-15 17:35_

@kbknapp If there were ever an example of a low priority bug, this would be it. :-) Thank you all the same though!

---

_Label `bug` added by @BurntSushi on 2017-05-30 11:39_

---

_Closed by @BurntSushi on 2017-07-30 22:05_

---
