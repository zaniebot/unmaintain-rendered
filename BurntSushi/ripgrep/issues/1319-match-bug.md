```yaml
number: 1319
title: match bug
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2019-07-05T17:13:40Z
updated_at: 2020-02-17T22:16:39Z
url: https://github.com/BurntSushi/ripgrep/issues/1319
synced_at: 2026-01-12T16:13:23Z
```

# match bug

---

_@BurntSushi_

```
$ rg --version
ripgrep 11.0.1 (rev 7bf7ceb5d3)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

This matches:

```
$ echo 'CCAGCTACTCGGGAGGCTGAGGCTGGAGGATCGCTTGAGTCCAGGAGTTC' | egrep 'CCAGCTACTCGGGAGGCTGAGGCTGGAGGATCGCTTGAGTCCAGGAG[ATCG]{2}C'
CCAGCTACTCGGGAGGCTGAGGCTGGAGGATCGCTTGAGTCCAGGAGTTC
```

But this doesn't:

```
$ echo 'CCAGCTACTCGGGAGGCTGAGGCTGGAGGATCGCTTGAGTCCAGGAGTTC' | rg 'CCAGCTACTCGGGAGGCTGAGGCTGGAGGATCGCTTGAGTCCAGGAG[ATCG]{2}C'
```

To minimize, this doesn't match:

```
$ rg 'TTGAGTCCAGGAG[ATCG]{2}C' /tmp/subject
```

But this does:

```
$ rg 'TGAGTCCAGGAG[ATCG]{2}C' /tmp/subject
1:CCAGCTACTCGGGAGGCTGAGGCTGGAGGATCGCTTGAGTCCAGGAGTTC
```

The only difference between the latter two is that the latter removes the first
`T` from the regex.

From inspecting the `--trace` output, I note that from the former regex, it
says this:

```
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:105: required literals found: [Complete(TTGAGTCCAGGAGAC), Complete(TTGAGTCCAGGAGCC), Complete(TTGAGTCCAGGAGGC), Complete(TTGAGTCCAGGAGTC)]
TRACE|grep_regex::matcher|grep-regex/src/matcher.rs:52: extracted fast line regex: (?-u:TTGAGTCCAGGAGAC|TTGAGTCCAGGAGCC|TTGAGTCCAGGAGGC|TTGAGTCCAGGAGTC)
```

But in the latter regex (the one that works), we have this:

```
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:59: literal prefixes detected: Literals { lits: [Complete(TGAGTCCAGGAGAAC), Complete(TGAGTCCAGGAGCAC), Complete(TGAGTCCAGGAGGAC), Complete(TGAGTCC
AGGAGTAC), Complete(TGAGTCCAGGAGACC), Complete(TGAGTCCAGGAGCCC), Complete(TGAGTCCAGGAGGCC), Complete(TGAGTCCAGGAGTCC), Complete(TGAGTCCAGGAGAGC), Complete(TGAGTCCAGGAGCGC), Complete(TGAGTCCAGGAGGGC)
, Complete(TGAGTCCAGGAGTGC), Complete(TGAGTCCAGGAGATC), Complete(TGAGTCCAGGAGCTC), Complete(TGAGTCCAGGAGGTC), Complete(TGAGTCCAGGAGTTC)], limit_size: 250, limit_class: 10 }
```

Therefore, this is almost certainly a bug in literal extraction. Moreover,
this Rust program correctly prints `true`:

```rust
fn main() {
    let pattern = r"CCAGCTACTCGGGAGGCTGAGGCTGGAGGATCGCTTGAGTCCAGGAG[ATCG]{2}C";
    let haystack = "CCAGCTACTCGGGAGGCTGAGGCTGGAGGATCGCTTGAGTCCAGGAGTTC";

    let re = regex::Regex::new(pattern).unwrap();
    println!("{:?}", re.is_match(haystack));
}
```

Which points the finger at `grep-regex`'s inner literal extraction. Sigh.

---

_Label `bug` added by @BurntSushi on 2019-07-05 17:13_

---

_Comment by @BurntSushi on 2019-07-06 14:58_

Oh, forgot to mention that this was originally reported in this StackOverflow question: https://stackoverflow.com/questions/56906725/ripgrep-missing-character-class-repetions

---

_Comment by @jakubadamw on 2019-09-05 21:30_

Replacing the line:

https://github.com/BurntSushi/ripgrep/blob/4858267f3b97fe2823d2ce104c1f90ec93eee8d7/grep-regex/src/literal.rs#L217

with

```rust
if max.map_or(true, |max| min <= max) {
```

or just dropping the whole `if` (as `min > max` never holds) condition fixes the issue for me. That condition looked suspicious to me at a first glance but as I do not understand that code very well, I sadly cannot substantiate why this is the *right* fix and I'm not even convinced it is.

---

_Comment by @jakubadamw on 2019-09-06 20:59_

I wrote [a fuzzer](https://github.com/jakubadamw/grep-searcher-fuzz-test/blob/ab50e73ac26c5f175b228a235ad16957bfc87321/src/main.rs) that uses the [regex_generate](https://crates.io/crates/regex_generate) crate to produce matching inputs for a given fuzz-generated regular expression and then checks if `grep_matcher::RegexMatcher` successfully matches that input against the regex. It has successfully found the class of errors represented by this issue and with the proposed change applied it has not found any more failing cases yet.

---

_Comment by @BurntSushi on 2019-09-06 21:09_

@jakubadamw That's awesome! Do you want to submit a PR? If not, I'll get to this eventually!

---

_Comment by @jakubadamw on 2019-09-06 21:46_

@BurntSushi, sure! 🙂

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
