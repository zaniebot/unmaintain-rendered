```yaml
number: 1625
title: "--threads on multiple files gives non repeteable results"
type: issue
state: closed
author: marcin-github
labels:
  - invalid
assignees: []
created_at: 2020-06-22T17:15:30Z
updated_at: 2020-06-22T17:27:47Z
url: https://github.com/BurntSushi/ripgrep/issues/1625
synced_at: 2026-01-12T16:13:23Z
```

# --threads on multiple files gives non repeteable results

---

_@marcin-github_

#### What version of ripgrep are you using?

ripgrep 12.0.1 (rev 1d5b1011e5)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

apt install ripgrep

#### What operating system are you using ripgrep on?

Ubuntu 18.04.4 LTS

#### Describe your bug.

When I run rg with multiple threads n many files I get unreliable, almost random values. 

#### What are the steps to reproduce the behavior?

Create a couple files with text and run
rg -j 5 -e "foo" textfiles*
change number of threads and try again.

#### What is the actual behavior?
When I run ripgrep with `-j 1` it's ok:
```
$ rg -z -I -j 1  -v -e "foo"  log-{,xx-}2020-06-1*  | wc -l
65273773
```
with two threads:
```
$ rg -z -I -j 2  -v -e "foo"  log-{,xx-}2020-06-1*  | wc -l
65273773
```
three:
```
$ rg -z -I -j 3  -v -e "foo"  log-{,xx-}2020-06-1*  | wc -l
38149072
```
eight:
```
$ rg -z -I -j 8  -v -e "foo"  log-{,xx-}2020-06-1*  | wc -l
0
```

with --debug:
```
$ rg --debug -z -I -j 2  -v -e "foo"  log-{,xx-}2020-06-1*  | wc -l
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(foo)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
65273773
```
```
$ rg --debug -z -I -j 8  -v -e "foo"  log-{,xx-}2020-06-1*  | wc -l
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(foo)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
0
```


#### What is the expected behavior?

I expect to have the same number of lines even with one hundread threads:)


---

_Comment by @marcin-github on 2020-06-22 17:20_

Invalid bug. OOM  kills rg.

---

_Closed by @marcin-github on 2020-06-22 17:20_

---

_Label `invalid` added by @BurntSushi on 2020-06-22 17:27_

---
