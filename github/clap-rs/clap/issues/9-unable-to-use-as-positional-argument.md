---
number: 9
title: "Unable to use `-` as positional argument"
type: issue
state: closed
author: jhelwig
labels:
  - C-bug
assignees: []
created_at: 2015-03-17T01:15:10Z
updated_at: 2018-08-02T03:29:36Z
url: https://github.com/clap-rs/clap/issues/9
synced_at: 2026-01-07T13:12:19-06:00
---

# Unable to use `-` as positional argument

---

_Issue opened by @jhelwig on 2015-03-17 01:15_

Working on an app that should be able to operate on either STDIN or a file, I was trying to use `-` as the file name to indicate STDIN (as is the convention used in the app I'm porting, and elsewhere), but that causes an index out of bounds panic.

```
% ./target/debug/signing-participants -
thread '<main>' panicked at 'index out of bounds: the len is 0 but the index is 0', /Users/rustbuild/src/rust-buildbot/slave/nightly-dist-rustc-mac/build/src/libcore/str/mod.rs:1685
```

So, I thought I'd try the common `--` to indicate "only positional arguments follow", but that fails as well but much more gracefully.

```
% ./target/debug/signing-participants -- -
Argument -- isn't valid
USAGE:
    signing-participants [FLAGS] [OPTIONS] <INPUT>
For more information try --help
```


---

_Comment by @kbknapp on 2015-03-17 01:25_

Ok, I think I've got an idea of how I can make this work. I was using `-` and `--` to narrow down the searches to "flag" or "option", but I could do a check where if `-` is the only character in the argument, it'll parse as positional, as well as implement the `--` meaning only positional follows (which isn't currently implemented). I'll work on it tomorrow and post back with how it goes. 

Thanks for the heads up!


---

_Label `bug` added by @kbknapp on 2015-03-17 01:26_

---

_Closed by @kbknapp on 2015-03-17 02:52_

---

_Comment by @kbknapp on 2015-03-17 02:54_

Turned out to be an easier fix than anticipated. You can download v0.4.4 from crates.io now. If it's still not fixed just let me know. 

Note that `--` still isn't implemented, that's on the TODO list.


---

_Comment by @jhelwig on 2015-03-17 02:58_

0.4.4 works with `-` as a positional argument for me now.  Thanks for the quick response!  I'll keep an eye on #10 as that'll be nice to have, too.


---

_Referenced in [clap-rs/clap#3087](../../clap-rs/clap/issues/3087.md) on 2021-12-09 04:55_

---
