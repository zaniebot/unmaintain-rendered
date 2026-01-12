```yaml
number: 1210
title: Certain ignore patterns are affecting whether others are respected
type: issue
state: closed
author: dpatti
labels:
  - bug
assignees: []
created_at: 2019-03-06T16:18:29Z
updated_at: 2019-03-06T16:46:28Z
url: https://github.com/BurntSushi/ripgrep/issues/1210
synced_at: 2026-01-12T16:13:23Z
```

# Certain ignore patterns are affecting whether others are respected

---

_@dpatti_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

This is the distributed 0.10.0 release for `x86_64-unknown-linux-musl`

#### What operating system are you using ripgrep on?

CentOS Linux 7.6.1810

#### If this is a bug, what are the steps to reproduce the behavior?

This will reproduce the issue on my machine:

```
$ echo foo > foo.bc.js
$ rg -g '!*.bc.js' --no-ignore foo
$ rg -g '!*.bc.js' -g '!*.build_info.c' --no-ignore foo
$ rg -g '!*.build_info.c' -g '!*.bc.js' --no-ignore foo
foo.bc.js
1:foo
$ rg -g '!*.bald_info.c' -g '!*.bc.js' --no-ignore foo
$
```

This was originally discovered with `.ignore` files, but it looks like it extends to `-g` flags as well. What is happening here is: I would expect the `foo.bc.js` file we created to be ignored like it is in most of these invocations. However specifically the third one where we specify `!*.build_info.c` first, it is *not* ignored. It seems like certain strings will trigger this, and some will not, as the fourth invocation demonstrates.

Here's the debug output from the failing command:

```
$ rg -g '!*.build_info.c' -g '!*.bc.js' --no-ignore foo --debug
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(FOO), Complete(fOO), Complete(FoO), Complete(foO), Complete(FOo), Complete(fOo), Complete(Foo), Complete(foo)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 2 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
foo.bc.js
1:foo
```

---

_Comment by @BurntSushi on 2019-03-06 16:46_

This appears to be fixed on master. My guess is that this was due to a literal matching bug in the regex engine (which powers globs), but I'm not certain. Others are welcome to do a bisect to find the exact place where this was fixed. :-)

---

_Closed by @BurntSushi on 2019-03-06 16:46_

---

_Label `bug` added by @BurntSushi on 2019-03-06 16:46_

---
