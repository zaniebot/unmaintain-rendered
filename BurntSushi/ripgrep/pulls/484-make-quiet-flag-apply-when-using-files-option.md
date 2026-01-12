```yaml
number: 484
title: Make --quiet flag apply when using --files option
type: pull_request
state: merged
author: tiehuis
labels: []
assignees: []
merged: true
base: master
head: issue-483
created_at: 2017-05-19T12:17:37Z
updated_at: 2017-05-20T00:09:47Z
url: https://github.com/BurntSushi/ripgrep/pull/484
synced_at: 2026-01-12T18:23:13Z
```

# Make --quiet flag apply when using --files option

---

_@tiehuis_

Fixes #483.

Self-explanatory, but example behavior as follows (in ripgrep base directory).

```
$ (issue-483) cargo run -- --quiet --files --glob '*.tom'; echo "$?" 
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/rg --quiet --files --glob '*.tom'`
1
$ (issue-483) cargo run -- --quiet --files --glob '*.toml'; echo "$?" 
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/rg --quiet --files --glob '*.toml'`
0
```

---

_@BurntSushi requested changes on 2017-05-19 12:21_

Looks great to me, thanks! Could you please add a regression test for this in `tests/tests.rs`? Just follow the example of other regression tests. Thank you. :-)

---

_Comment by @BurntSushi on 2017-05-19 12:38_

The appveyor failures look spurious... sigh... Don't worry about them. :-)

---

_Comment by @tiehuis on 2017-05-19 12:56_

Sure thing. Will push an update within the next day.

---

_Comment by @tiehuis on 2017-05-19 23:25_

Added two tests. One which tests that the quiet flag is working in the case of a match, and the other which tests that an error exit code is being propagated correctly.

---

_@BurntSushi approved on 2017-05-19 23:56_

---

_Comment by @BurntSushi on 2017-05-20 00:00_

Fantastic! Very nicely done. :-)

---

_Merged by @BurntSushi on 2017-05-20 00:00_

---

_Closed by @BurntSushi on 2017-05-20 00:00_

---

_Branch deleted on 2017-05-20 00:09_

---
