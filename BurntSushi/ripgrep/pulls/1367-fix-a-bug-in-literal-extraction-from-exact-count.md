```yaml
number: 1367
title: Fix a bug in literal extraction from exact-count repetition patterns
type: pull_request
state: closed
author: jakubadamw
labels:
  - rollup
assignees: []
base: master
head: issue-1319
created_at: 2019-09-06T21:26:03Z
updated_at: 2020-02-17T22:16:39Z
url: https://github.com/BurntSushi/ripgrep/pull/1367
synced_at: 2026-01-12T18:23:13Z
```

# Fix a bug in literal extraction from exact-count repetition patterns

---

_@jakubadamw_

Closes #1319.

---

_Comment by @jakubadamw on 2019-09-06 21:31_

I'll honestly say that I am not sure if this is the right fix. I just don't know the code. But this particular line looked suspicious.

I did try to verify this empirically by writing [a fuzzer](https://github.com/jakubadamw/grep-searcher-fuzz-test/blob/master/src/main.rs) to look out for more bugs by fuzz-feeding the `grep_matcher::RegexMatcher` with inputs derived from a particular regular expression with the [regex_generate](https://crates.io/crates/regex_generate) crate, i.e. ones that by definition ought to match. As of this comment the fuzzer has not found any more failing cases (and it had found the one described in the bug). However, that still is no proof that this fix is correct.

---

_Comment by @jakubadamw on 2019-09-06 21:44_

One of the failing fuzz-generated cases of the sort related to this issue:

```
(lldb) p needle
(alloc::string::String) $0 = "TAACCCCGTTGTACAGTTGCAGCCGCA"
(lldb) p haystack
(alloc::string::String) $1 = "TTTTTTTTTTAACCCCGTTGTACAGTTGCAGCCGCATATTTTTTTTT"
(lldb) p regex_string
(alloc::string::String) $2 = "TAA[ATCG]{5,5}TTGTACAGTTGCAGCCGCA"
```

---

_Label `rollup` added by @BurntSushi on 2020-02-17 18:14_

---

_@BurntSushi approved on 2020-02-17 18:17_

Apologies for the delay, but thanks so much for this PR! I agree that this does fix the problem. I simplified this just a bit by just removing the `if n < min {` check above and uncondionally running `lits.cut()`.

This code is completely inscrutable, but the essence of the problem is that the literal detector was continuing the extension of literals when it shouldn't have been.

I'll be merging this in #1486, where your commit message has grown a few details if you're curious.

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
