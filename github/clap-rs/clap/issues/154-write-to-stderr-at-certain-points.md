---
number: 154
title: Write to stderr at certain points
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
assignees: []
created_at: 2015-07-09T19:30:09Z
updated_at: 2018-08-02T03:29:40Z
url: https://github.com/clap-rs/clap/issues/154
synced_at: 2026-01-07T13:12:19-06:00
---

# Write to stderr at certain points

---

_Issue opened by @kbknapp on 2015-07-09 19:30_

Some light reading about [stderr](http://www.jstorimer.com/blogs/workingwithcode/7766119-when-to-use-stderr-instead-of-stdout)

Currently everything is spit to `stdout`.

One thing I'm unsure about is identifying the process, like I'd like to do:

```
$ myprog --some args
myprog: unrecognized argument '--some'

USAGE:
    myprog [FLAGS] [ARGS]

For more information try --help
```

But the question is, do I use the `bin_name` or `name`. I'm leaning towards `bin_name` as that's typically the standard.

This also mean's `myprog: some error` would replace the current `error: some error` format.


---

_Label `enhancement` added by @kbknapp on 2015-07-09 19:30_

---

_Referenced in [clap-rs/clap#155](../../clap-rs/clap/pulls/155.md) on 2015-07-11 15:24_

---

_Closed by @kbknapp on 2015-07-11 15:28_

---
