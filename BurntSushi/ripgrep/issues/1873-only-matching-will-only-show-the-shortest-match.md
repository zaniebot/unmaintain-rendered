```yaml
number: 1873
title: "--only-matching will only show the shortest match for the input "
type: issue
state: closed
author: lpetre
labels:
  - question
assignees: []
created_at: 2021-05-26T11:27:20Z
updated_at: 2021-05-26T12:50:21Z
url: https://github.com/BurntSushi/ripgrep/issues/1873
synced_at: 2026-01-12T16:13:24Z
```

# --only-matching will only show the shortest match for the input 

---

_@lpetre_

#### What version of ripgrep are you using?
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?
Via the latest nix package

#### What operating system are you using ripgrep on?

> uname -a
Linux c126593d20c7 5.4.0-1043-gcp #46-Ubuntu SMP Mon Apr 19 19:17:04 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux

#### Describe your bug.

If I ask ripgrep to find two strings, where the first string is a substring of the other, and use --only-matching, only the shorter string is printed.

If you pass the longer string first, you get the expected output.

See a simple repo here: https://replit.com/@lpetre/ripgrep-issue

#### What are the steps to reproduce the behavior?
```
lpetre@localhost:rg-bug $ cat input.txt 
this.is.a.string
this.is.a.string.too
lpetre@localhost:rg-bug $ rg -f input.txt -o input.txt 
1:this.is.a.string
2:this.is.a.string
lpetre@localhost:rg-bug $ grep -f input.txt -o input.txt 
this.is.a.string
this.is.a.string.too
lpetre@localhost:rg-bug $ rg -e this.is.a.string.too -e this.is.a.string -o input.txt 
1:this.is.a.string
2:this.is.a.string.too
```
#### What is the actual behavior?
```
lpetre@localhost:rg-bug $ rg -e this.is.a.string.too -e this.is.a.string -o input.txt  --debug
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:115: required literal found: "this"
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
1:this.is.a.string
2:this.is.a.string.too
```

```
lpetre@localhost:rg-bug $ rg -f input.txt -o input.txt --debug
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:115: required literal found: "this"
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
1:this.is.a.string
2:this.is.a.string
```
#### What is the expected behavior?
I think ripgrep should show the longer result regardless of the order of the input.
```
lpetre@localhost:rg-bug $ grep -f input.txt -o input.txt 
this.is.a.string
this.is.a.string.too
```

---

_Comment by @BurntSushi on 2021-05-26 12:50_

This result here correct behavior and intended, although your description of it is incorrect. ripgrep does not print the shortest match. It prints the _first_ match. In contrast, POSIX grep will always show the longest match. ripgrep implements leftmost-first or "preference order" matching, which corresponds to how backtracking regex engines report matches. If you flip the order of your patterns, you can see how preference order works:

```
$ cat /tmp/input.txt
this.is.a.string.too
this.is.a.string
$ rg -f /tmp/input.txt -o /tmp/input.txt
1:this.is.a.string.too
2:this.is.a.string
```

Indeed, given patterns p1 and p2, where p1 comes before p2 in the pattern list and where p1 is a prefix of p2, it follows that p2 will _never_ be reported as a match.

Because of this property, if your patterns are simple literals, and if you sort them in descending order by length, then it will report precisely the same results as POSIX grep.

It is plausible that ripgrep will gain "proper" support for POSIX leftmost-longest semantics in the future, but it's likely years away from happening.

---

_Closed by @BurntSushi on 2021-05-26 12:50_

---

_Label `question` added by @BurntSushi on 2021-05-26 12:50_

---

_Locked by @ghost on 2021-05-26 12:50_

---
