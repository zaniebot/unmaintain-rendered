```yaml
number: 1331
title: "hang when greping /sys/kernel/security/apparmor/{policy,revision}"
type: issue
state: closed
author: hongxuchen
labels:
  - wontfix
assignees: []
created_at: 2019-07-28T16:06:53Z
updated_at: 2019-07-28T19:07:29Z
url: https://github.com/BurntSushi/ripgrep/issues/1331
synced_at: 2026-01-12T16:13:23Z
```

# hang when greping /sys/kernel/security/apparmor/{policy,revision}

---

_@hongxuchen_

#### What version of ripgrep are you using?

```
ripgrep 11.0.1 (rev 08ae4da2b7)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?
`cargo build --release` against github repo, HEAD version

#### What operating system are you using ripgrep on?
`Linux H5 5.0.0-13-generic #14-Ubuntu SMP Mon Apr 15 14:59:14 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux`, Ubuntu 19.04

#### Describe your question, feature request, or bug.

#### If this is a bug, what are the steps to reproduce the behavior?

run `rg power /sys/kernel/security/apparmor/policy` or `rg power /sys/kernel/security/apparmor/revision` (or explicitly with `--no-follow`), it hangs there.

#### If this is a bug, what is the actual behavior?

```
$ rg --no-follow power /sys/kernel/security/apparmor/revision --debug
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:59: literal prefixes detected: Literals { lits: [Complete(power)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|grep_searcher::searcher::mmap|grep-searcher/src/searcher/mmap.rs:86: /sys/kernel/security/apparmor/revision: failed to open memory map: memory map must have a non-zero length
# hang there
```

#### If this is a bug, what is the expected behavior?

I expect that `rg` would exit soon (especially for `/sys/kernel/security/apparmor/revision`; but `grep` would also hang on this zero-length file) or at least behave like `grep` for `/sys/kernel/security/apparmor/policy`:
```
$ grep power -r /sys/kernel/security/apparmor/policy
# exit soon

$ grep power -R /sys/kernel/security/apparmor/policy
# hang
```

Other information:

```
$ ls -l /sys/kernel/security/apparmor/
total 0
drwxr-xr-x  2 root root 0 Jul 27 19:54 attr/
drwxr-xr-x 15 root root 0 Jul 27 19:54 features/
lr--r--r--  1 root root 0 Jul 27 19:54 policy -> 'apparmorfs:[12192]'
-r--r--r--  1 root root 0 Jul 27 19:54 profiles
-r--r--r--  1 root root 0 Jul 27 19:54 revision
```

#### What do you think ripgrep should have done?

This seems to be specific to `apparmor` and I've no idea what is going on.

---

_Comment by @BurntSushi on 2019-07-28 18:46_

`grep` hangs when running

```
$ grep power /sys/kernel/security/apparmor/revision
```

Indeed, `cat` hangs too:

```
$ cat /sys/kernel/security/apparmor/revision
```

As such, this isn't a problem with ripgrep. `strace` (in all instances) indicates that the process hangs in the `read` call.

A little googling [demystifies this](https://gitlab.com/apparmor/apparmor/wikis/AppArmorInterfaces#syskernelsecurityapparmorrevision):

> The revisions file behaves like a pipe with the current revision
> available to read when the file is opened. Subsequent reads will
> sleep the reading task until the next revision is available.

So this seems like intended behavior. In general, you shouldn't be arbitrarily searching the `/sys`, `/proc` and `/dev` directories. Other issues have come up on this, since folks seem to assume that these directories are normal file hierarchies. But they aren't.

---

_Closed by @BurntSushi on 2019-07-28 18:46_

---

_Label `wontfix` added by @BurntSushi on 2019-07-28 18:46_

---

_Comment by @hongxuchen on 2019-07-28 18:55_

Yes, I noticed that `grep` (as well as `cat`) hangs on the revision file. Is there any way to avoid hangs when greping recursively, such as what `grep -r` does? I met several scenarios to search inside special directories in Linux.

---

_Comment by @BurntSushi on 2019-07-28 19:07_

I don't know. Someone will need to dig into what `grep` does and figure out what its case analysis looks like. I don't have any plans to do that myself. If someone wants to do that research and post it back here, then I can consider re-opening this.

---
