```yaml
number: 104
title: Allow (and ignore) whitespace-only lines in .gitignore files
type: pull_request
state: closed
author: ddrcoder
labels: []
assignees: []
base: master
head: gitignore_blank_lines
created_at: 2016-09-26T17:07:29Z
updated_at: 2016-09-27T21:59:51Z
url: https://github.com/BurntSushi/ripgrep/pull/104
synced_at: 2026-01-12T18:23:12Z
```

# Allow (and ignore) whitespace-only lines in .gitignore files

---

_@ddrcoder_

Git considers these to be blank lines.


---

_Comment by @ddrcoder on 2016-09-26 17:08_

Without this, it panics at the `..unwrap() == '/'` on line 301.


---

_Comment by @BurntSushi on 2016-09-26 20:54_

@ddrcoder Thanks! This looks OK, but could you add a regression test please?


---

_Comment by @BurntSushi on 2016-09-26 22:56_

I merged this in https://github.com/BurntSushi/ripgrep/commit/ebabe1df6a5922e88aad9024661aaf3a456200b1 with a regression test. Sorry about being impatient, but I wanted to get this bug fixed with a new release out tonight. Thanks so much for catching this and fixing it!


---

_Closed by @BurntSushi on 2016-09-26 22:56_

---

_Comment by @ddrcoder on 2016-09-27 21:59_

Sorry I couldn't get back to this yesterday. I'm glad you didn't wait on my account. :)


---
