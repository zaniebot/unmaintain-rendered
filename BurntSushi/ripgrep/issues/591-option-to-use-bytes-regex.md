```yaml
number: 591
title: Option to use bytes regex?
type: issue
state: closed
author: lespea
labels: []
assignees: []
created_at: 2017-08-31T17:09:37Z
updated_at: 2017-08-31T23:19:12Z
url: https://github.com/BurntSushi/ripgrep/issues/591
synced_at: 2026-01-12T16:13:22Z
```

# Option to use bytes regex?

---

_@lespea_

Sorry if this already exists and I just can't find it.  I have an issue where I'm piping a large amount of data into rg and it's not always valid unicode (could be a mix of encodings in the same stream).  I tried using the encoding ascii but the regex will still complain that the input isn't valid utf8.  Is there (or could there be) a way to use the bytes::Regex instead of the normal Regex to get around this issue?

---

_Comment by @BurntSushi on 2017-08-31 19:48_

Sorry, but the way you've described this issue doesn't make any sense to me. Please provide a way for me to reproduce your problem.

bytes::Regex is a red herring. That's what ripgrep already uses.

---

_Comment by @lespea on 2017-08-31 23:19_

Well sorry to waste your time but I completely misread what the `-f` option did. My apologies!

---

_Closed by @lespea on 2017-08-31 23:19_

---
