```yaml
number: 473
title: Search lines matching two patterns or more
type: issue
state: closed
author: rik
labels:
  - duplicate
assignees: []
created_at: 2017-05-04T14:25:36Z
updated_at: 2018-04-25T20:55:30Z
url: https://github.com/BurntSushi/ripgrep/issues/473
synced_at: 2026-01-12T16:13:22Z
```

# Search lines matching two patterns or more

---

_@rik_

`git grep` supports `--and` that allows you to match against all patterns instead of any pattern. Right now, I'm doing `rg pattern1|rg pattern2` but that gives a way less readable output than `rg --heading -e pattern1 --and -e pattern2` would.

---

_Comment by @BurntSushi on 2017-05-04 14:36_

It never ceases to amaze me how fundamentally opposed the Unix philosophy is to the nice colored output that tools like ack, ag and ripgrep use by default.

I reluctantly accept that this is desirable.

---

_Label `enhancement` added by @BurntSushi on 2017-05-04 14:36_

---

_Label `question` added by @BurntSushi on 2017-05-04 14:36_

---

_Comment by @BurntSushi on 2017-05-04 14:37_

This ticket needs a more detailed specification before anyone should attempt to work on it. In particular, the interaction with other flags (like `--replace`, `-o`, etc.) needs to be ironed out.

---

_Comment by @kpp on 2017-06-01 13:05_

W8, what about `--or`? Are you going to give the user the power of the syntax of `find`?

---

_Comment by @BurntSushi on 2017-06-01 13:16_

@kpp What's wrong with `re1|re2`?

> Are you going to give the user the power of the syntax of find?

No. ripgrep is not a `find` replacement.

---

_Label `icebox` added by @BurntSushi on 2017-10-22 01:44_

---

_Comment by @BurntSushi on 2018-04-25 20:55_

This is a duplicate of #875. Yeah, this ticket came first, but I've written more in #875, so I'm going to track this feature there.

---

_Closed by @BurntSushi on 2018-04-25 20:55_

---

_Label `duplicate` added by @BurntSushi on 2018-04-25 20:55_

---

_Label `enhancement` removed by @BurntSushi on 2018-04-25 20:55_

---

_Label `icebox` removed by @BurntSushi on 2018-04-25 20:55_

---

_Label `question` removed by @BurntSushi on 2018-04-25 20:55_

---
