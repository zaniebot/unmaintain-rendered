```yaml
number: 871
title: "Support !directory to exclude a dir from consideration?"
type: issue
state: closed
author: mqudsi
labels:
  - question
assignees: []
created_at: 2018-03-29T13:21:43Z
updated_at: 2018-03-29T15:00:03Z
url: https://github.com/BurntSushi/ripgrep/issues/871
synced_at: 2026-01-12T16:13:22Z
```

# Support !directory to exclude a dir from consideration?

---

_@mqudsi_

@BurntSushi Would you be open to having `rg PATTERN !DIR [!DIR2 ..]` be used to exclude a directory from consideration at the command line?

---

_Comment by @BurntSushi on 2018-03-29 13:23_

No. I've never seen another tool do this, and I would prefer not to continue the spread of the use of `!` in command line arguments.

---

_Label `question` added by @BurntSushi on 2018-03-29 13:24_

---

_Comment by @mqudsi on 2018-03-29 13:31_

Thanks for the quick reply. What about an option to exclude a path, then (without `-g !`) (a la many tools' `-x PATH` option)?

---

_Comment by @BurntSushi on 2018-03-29 13:37_

@mqudsi Well, yes, I never would have used `!` if it were simple to add a `-G` (negation of `-g`) flag. :-) That work is tracked in #809, and I believe it's now possible to implement given improvements in the latest release of `clap` (argv parser). I'll _probably_ get around to it in the next release.

---

_Comment by @mqudsi on 2018-03-29 15:00_

Thanks :)

---

_Closed by @mqudsi on 2018-03-29 15:00_

---
