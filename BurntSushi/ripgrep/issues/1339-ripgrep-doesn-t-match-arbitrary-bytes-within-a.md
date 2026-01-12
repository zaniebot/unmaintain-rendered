```yaml
number: 1339
title: "ripgrep doesn't match arbitrary bytes within a file"
type: issue
state: closed
author: keysmashes
labels:
  - doc
assignees: []
created_at: 2019-08-05T15:17:24Z
updated_at: 2020-05-09T03:24:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1339
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep doesn't match arbitrary bytes within a file

---

_@keysmashes_

#### What version of ripgrep are you using?

ripgrep 11.0.1 (rev 1f1cd9b467)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

GitHub deb, I think

#### What operating system are you using ripgrep on?

Ubuntu 18.04

#### Describe your question, feature request, or bug.

grep finds arbitrary bytes within a binary file, but ripgrep does not.

#### If this is a bug, what are the steps to reproduce the behavior?

```console
$ grep $'\xa7' <(printf '\xa7')
Binary file /dev/fd/63 matches
$ echo $?
0
$ rg -uuu --text --binary '\xa7' <(printf '\xa7')
$ echo $?
1
```

#### If this is a bug, what is the actual behavior?

```console
$ rg -uuu --text --binary --debug '\xa7' <(printf '\xa7')
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:59: literal prefixes detected: Literals { lits: [Complete(§)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

#### If this is a bug, what is the expected behavior?

Either ripgrep should be able to search for arbitrary bytes, or it should not print a message implying that it can do so:

```console
$ rg $'\xa7'
found invalid UTF-8 in pattern at byte offset 0 (use hex escape sequences to match arbitrary bytes in a pattern, e.g., \xFF): '\xA7'
```

---

_Comment by @BurntSushi on 2019-08-05 16:30_

By default, patterns are Unicode aware, so all escape sequences identify the codepoint to search for, which is not the same as the byte to search for. To search for a raw byte, you need to disable Unicode. For example, `(?-u:\xa7)` should do what you want. This is documented in the syntax docs linked from the man page.

The error message is indeed wrong, or at least, incomplete.

---

_Label `doc` added by @BurntSushi on 2019-08-05 16:31_

---

_Closed by @BurntSushi on 2020-05-09 03:24_

---
