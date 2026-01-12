```yaml
number: 1941
title: result and error output mixing up
type: issue
state: closed
author: kpcyrd
labels:
  - bug
  - rollup
assignees: []
created_at: 2021-07-20T10:09:50Z
updated_at: 2023-07-08T22:53:13Z
url: https://github.com/BurntSushi/ripgrep/issues/1941
synced_at: 2026-01-12T16:13:24Z
```

# result and error output mixing up

---

_@kpcyrd_

#### What version of ripgrep are you using?

```
% rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Arch Linux

#### What operating system are you using ripgrep on?

Arch Linux

#### Describe your bug.

Searching in a folder that contains files/folders that I don't have permissions to causes the errors to mix up with the results.

#### What are the steps to reproduce the behavior?

You need to search in a folder with some files that you can't access (like /etc), that also contains some files that match your search.

#### What is the actual behavior?

```
% rg pcp /etc 
/etc/shadow: Permission denied (os error 13)
/etc/gshadow: Permission denied (os error /etc/passwd
39:pcp:x:959:959:Performance Co-Pilot:/var/lib/pcp:/usr/bin/nologin
13)

/etc/shadow-/etc/group
72:pcp:x:959:
: Permission denied (os error 13)
/etc/gshadow-: Permission denied (os error 13)
/etc/named.conf: Permission denied (os error 13)
/etc/libaudit.conf: Permission denied (os error 13)
/etc/sudoers.pacnew: Permission denied (os error 13)
/etc/shadow.pacnew: Permission denied (os error 13)
/etc/crypttab: Permission denied (os error 13)
```

#### What is the expected behavior?

errors should likely be sent to the main thread for printing so they are in-order:

```
% rg pcp /etc 
/etc/shadow: Permission denied (os error 13)
/etc/gshadow: Permission denied (os error 13)
/etc/passwd
39:pcp:x:959:959:Performance Co-Pilot:/var/lib/pcp:/usr/bin/nologin

/etc/group
72:pcp:x:959:
/etc/shadow-: Permission denied (os error 13)
/etc/gshadow-: Permission denied (os error 13)
/etc/named.conf: Permission denied (os error 13)
/etc/libaudit.conf: Permission denied (os error 13)
/etc/sudoers.pacnew: Permission denied (os error 13)
/etc/shadow.pacnew: Permission denied (os error 13)
/etc/crypttab: Permission denied (os error 13)
```



---

_Comment by @BurntSushi on 2021-07-20 13:37_

Hmmm yes, I can indeed reproduce this. A quick work-around is to disable parallelism with `-j1`.

In terms of fixing this, it isn't quite as simple as "send errors to the main thread." Errors are (or should be) already bubbled up to the top-level routines in `crates/core/main.rs` and printed with `err_message!`. The main problem here is that we prevent interleaving on stdout by acquiring the std's stdout lock at a fairly low layer of abstraction (down inside of `termcolor`). That is, printing is itself done on different threads. Printing isn't done by the main thread. We might be able to "fake" it by just acquiring that same lock in `main.rs` when printing an error message. It's a bit weird because it's a glorious violation of abstraction, but I just tried it and it does appear to resolve the issue. I'll noodle on this a little bit.

---

_Label `bug` added by @BurntSushi on 2021-07-20 13:37_

---

_Label `rollup` added by @BurntSushi on 2023-07-08 21:41_

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---
