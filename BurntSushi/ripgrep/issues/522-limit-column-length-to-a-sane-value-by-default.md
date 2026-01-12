```yaml
number: 522
title: Limit column length to a sane value by default?
type: issue
state: closed
author: mqudsi
labels:
  - question
assignees: []
created_at: 2017-06-18T20:27:17Z
updated_at: 2017-10-22T01:28:55Z
url: https://github.com/BurntSushi/ripgrep/issues/522
synced_at: 2026-01-12T16:13:22Z
```

# Limit column length to a sane value by default?

---

_@mqudsi_

Would there be any interest in limiting the column length to a sane value by default so that machine-generated/machine-parsed ASCII files containing "lines" that are several MiB long do not take over the output?

e.g. how would you feel if `-M` defaulted to something like `80*24` (being the default terminal window size on many systems) with `-M 0` disabling this limit? 

---

_Renamed from "Limit context within line for long lines?" to "Limit context within line for long lines by default?" by @mqudsi on 2017-06-18 20:28_

---

_Renamed from "Limit context within line for long lines by default?" to "Limit context within line for long lines by default? [please delete]" by @mqudsi on 2017-06-18 20:28_

---

_Closed by @mqudsi on 2017-06-18 20:28_

---

_Renamed from "Limit context within line for long lines by default? [please delete]" to "Limit column length to a sane value by default?" by @mqudsi on 2017-06-18 20:40_

---

_Reopened by @mqudsi on 2017-06-18 20:43_

---

_Comment by @BurntSushi on 2017-06-18 21:08_

I think reasonable people could go either way on this. IMO, I kind of like the current behavior where the default is that you see everything. Since searching is (typically) so fast, it's typically not onerous to re-run the command with `-M100` (or whatever) if you happen to get spammed with a very long line. Of course, if you happen to see long lines very frequently in your searches, then perhaps `-M100` (or whatever) would be reasonable to enable by default. You can do that today by defining an alias in your shell, e.g., for bash that's `alias rg="rg -M100"`. The downside there is that `rg -M100 -M0` yields an error since the `-M` flag can't be specified more than once. So perhaps if we fixed that such that the most recent value "wins", then this would be a reasonable compromise?

---

_Label `question` added by @BurntSushi on 2017-06-18 21:08_

---

_Comment by @BurntSushi on 2017-10-22 01:28_

Coming back to this, I don't think we should be limiting column length by default. Uses can add something like `-M100` in their alias, and once #196 is done, users could add `-M0` to override the previous `-M100`.

---

_Closed by @BurntSushi on 2017-10-22 01:28_

---
