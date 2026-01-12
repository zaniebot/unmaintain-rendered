```yaml
number: 166
title: Error parsing regex
type: issue
state: closed
author: durka
labels:
  - bug
assignees: []
created_at: 2016-10-11T17:12:03Z
updated_at: 2016-10-13T10:39:48Z
url: https://github.com/BurntSushi/ripgrep/issues/166
synced_at: 2026-01-12T18:23:11Z
```

# Error parsing regex

---

_@durka_

I keep seeing "Error parsing regex", even if I pass -F. But the program continues anyway.

Example:

``` sh
$ rg -Ft rust Handle::new crates
Error parsing regex near '\\])$)' at character offset 28: Unicode features are not allowed when the Unicode (u) flag is not set.
crates/back/scribe/src/lib.rs
48:                let mut max_file = Handle::new();
49:                let mut max_pattern = Handle::new();
```


---

_Comment by @BurntSushi on 2016-10-11 17:18_

I wonder if it's coming from parsing a `gitignore` file. Are you using the latest version of `ripgrep`? (`0.2.2`.) If so, is there anyway for you to narrow down which `gitignore` file is causing this and share it? Does this error appear for all patterns that you try to search?


---

_Comment by @durka on 2016-10-11 17:25_

I have 0.2.1. There're a lot of gitignore files in this directory, is there any logging I can turn on to see which it is?


---

_Comment by @BurntSushi on 2016-10-11 17:29_

You can try `--debug`.
- Andrew

On Tue, Oct 11, 2016 at 1:25 PM, Alex Burka notifications@github.com
wrote:

> I have 0.2.1. There's a lot of gitignore files in this directory, is there
> any logging I can turn on to see which it is?
> 
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/166#issuecomment-252985494,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34upzM5059raBGnHQgwzrz84RRPRXks5qy8angaJpZM4KT4E8
> .


---

_Comment by @durka on 2016-10-11 17:30_

Error disappears with version 0.2.2.


---

_Closed by @durka on 2016-10-11 17:30_

---

_Comment by @BurntSushi on 2016-10-11 18:09_

Ah great, I fixed a few bugs around these errors in `0.2.2`.


---

_Label `bug` added by @BurntSushi on 2016-10-13 10:39_

---
