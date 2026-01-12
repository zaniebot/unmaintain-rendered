```yaml
number: 93
title: Possible bad literal extraction
type: issue
state: closed
author: brookst
labels:
  - bug
assignees: []
created_at: 2016-09-25T23:06:03Z
updated_at: 2016-09-26T00:11:41Z
url: https://github.com/BurntSushi/ripgrep/issues/93
synced_at: 2026-01-12T18:23:11Z
```

# Possible bad literal extraction

---

_@brookst_

I was trying to search for IP addresses and ran into an odd problem. I tried the expression `'(\d{1,3}\.){3}\d{1,3}'` and found no matches. I changed from a fixed repetition of the subgroup to one or more - `'(\d{1,3}\.)+\d{1,3}'` - and got matches. Using the `--debug` flag I see that the first case looks for the wrong literal:

```
DEBUG:grep::literals: required literal found: "..."
```

The second case only looks for a single `.` as expected.

Is this a bug in the regex parsing or am I doing something wrong?


---

_Label `bug` added by @BurntSushi on 2016-09-26 00:04_

---

_Comment by @BurntSushi on 2016-09-26 00:04_

It's a pretty sweet bug. This is indeed a bug in `ripgrep`'s inner literal extraction. (Over aggressive literal extraction is one of the largest sources of bugs in the regex engine too.)


---

_Closed by @BurntSushi on 2016-09-26 00:11_

---
