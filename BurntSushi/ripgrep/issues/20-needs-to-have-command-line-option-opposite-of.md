```yaml
number: 20
title: Needs to have command line option opposite of --with-filename
type: issue
state: closed
author: jikamens
labels:
  - enhancement
assignees: []
created_at: 2016-09-23T17:29:58Z
updated_at: 2016-09-24T23:24:41Z
url: https://github.com/BurntSushi/ripgrep/issues/20
synced_at: 2026-01-12T18:23:11Z
```

# Needs to have command line option opposite of --with-filename

---

_@jikamens_

There needs to be a way to disable the file name prefix even when multiple files are being searched.


---

_Comment by @BurntSushi on 2016-09-23 17:30_

Could you help motivate this by describing a use case? Thanks!


---

_Comment by @jikamens on 2016-09-23 17:32_

I sort of thought the use case was obvious: I want to see matches for a given pattern in a bunch of files but I don't care what files the matches are in. For example, if I want to search for log entries matching a pattern in a bunch of compressed log files which are split up only so they don't get too big, then each matching log line fully identifies everything I need to know about it in the log line itself; I don't care about the file name when looking at the results.


---

_Label `enhancement` added by @BurntSushi on 2016-09-23 17:43_

---

_Comment by @BurntSushi on 2016-09-23 17:43_

That's perfect, thank you. :-)


---

_Closed by @BurntSushi on 2016-09-24 23:24_

---

_Comment by @BurntSushi on 2016-09-24 23:24_

This is done. I added the `--no-filename` flag.


---
