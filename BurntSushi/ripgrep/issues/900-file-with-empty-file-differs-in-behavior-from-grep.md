```yaml
number: 900
title: "--file with empty file differs in behavior from grep"
type: issue
state: closed
author: mqudsi
labels:
  - enhancement
assignees: []
created_at: 2018-04-29T16:45:44Z
updated_at: 2018-07-22T16:47:22Z
url: https://github.com/BurntSushi/ripgrep/issues/900
synced_at: 2026-01-12T16:13:22Z
```

# --file with empty file differs in behavior from grep

---

_@mqudsi_

#### What version of ripgrep are you using?

0.8.1

#### What operating system are you using ripgrep on?

Linux/WSL

#### Describe your question, feature request, or bug.

From `man grep`:

> `-f FILE`, `--file=FILE`
> Obtain patterns from FILE, one per line.  If this option is used multiple times or is  combined  with  the  -e  (--regexp) option, search for all patterns given.  **The empty file contains zero patterns, and therefore matches nothing.**

rg's behavior here is to treat an empty file as matching everything instead:

```
touch empty.txt
rg -l -f empty.txt
# prints all files
```

See also #783

---

_Comment by @BurntSushi on 2018-04-29 16:55_

I don't understand what your goal is here. ripgrep differs from grep in many ways. Are you asking for better docs? A change in behavior?

---

_Comment by @mqudsi on 2018-04-29 17:04_

Sorry, I should have been clearer. I was requesting that the same handling of an empty file passed to `-f` be used. The logic is that rg may be programmatically invoked against a generated `matches.txt`, in which case there is value in treating an empty file as not matching anything.

---

_Label `enhancement` added by @BurntSushi on 2018-04-29 20:06_

---

_Comment by @BurntSushi on 2018-04-29 20:06_

OK, I agree. I had a chance to review #783 as well, and I agree that ripgrep should probably be consistent with grep here.

---

_Comment by @mqudsi on 2018-04-29 21:33_

Thanks, much appreciated!

---

_Closed by @BurntSushi on 2018-07-22 16:08_

---

_Comment by @mqudsi on 2018-07-22 16:47_

Thanks!

---
