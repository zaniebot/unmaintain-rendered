```yaml
number: 1546
title: How To Convert Case Using ripgrep
type: issue
state: closed
author: blueray453
labels:
  - duplicate
assignees: []
created_at: 2020-04-08T08:07:17Z
updated_at: 2020-04-08T11:56:29Z
url: https://github.com/BurntSushi/ripgrep/issues/1546
synced_at: 2026-01-12T16:13:23Z
```

# How To Convert Case Using ripgrep

---

_@blueray453_

I am using:
```
$ rg --version
ripgrep 12.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

$ rg  --pcre2-version
PCRE2 10.32 is available
JIT is available
```
https://www.regular-expressions.info/pcre2.html#subext says:

> Starting with version 10.21, PCRE2 offers an extended replacement string syntax that you can enable by including PCRE2_SUBSTITUTE_EXTENDED with the matching options when calling pcre2_substitute().
> ... 
> With PCRE2, any case conversion escape cancels the preceding escape. So you can’t combine them and even \u or \l will end a run of \U or \L.
> ...
> Case conversion runs through conditionals. Any case conversion in effect before the conditional also applies to the conditional. If the conditional contains its own case conversion escapes in the part of the conditional that is actually used, then those remain in effect after the conditional. So you could use ${1:+\U:\L}${2} to insert the text matched by the second capturing group in uppercase if the first group participated, and in lowercase if it didn’t.

So, what I understand from this is there is probably a way to convert case using `rg -P`. 

Something Like: `echo "test ING" | rg -P '[a-z]'  -r '\U\1'`

However It results in : `\U\1\U\1\U\1\U\1 ING`

So, I think `rg -P` is not understanding `\U`. Is it because extended replacement string syntax is not enabled? What can be done about that?

---

_Label `duplicate` added by @BurntSushi on 2020-04-08 11:03_

---

_Comment by @BurntSushi on 2020-04-08 11:04_

Duplicate of https://github.com/BurntSushi/ripgrep/issues/1538

---

_Closed by @BurntSushi on 2020-04-08 11:04_

---

_Comment by @blueray453 on 2020-04-08 11:37_

My issue was different though. Let me rephrase.

As per my understanding, `rg`'s `-P` let us use pcre2 regex engine.

PCRE2 10.21 onward offers an extended replacement string syntax.

My `--pcre2-version` is 10.32.

So, When I am using:

`echo "test ING" | rg -P '[a-z]' -r '\U\1'`

why is it  showing `\U\1\U\1\U\1\U\1 ING`?

> extended replacement string syntax that you can enable by including PCRE2_SUBSTITUTE_EXTENDED with the matching options when calling pcre2_substitute(). 

Have you not done that? If I want to do it, where shall I make the changes?

---

_Comment by @BurntSushi on 2020-04-08 11:56_

Yes, I know what you meant. But using the PCRE2 regex engine doesn't mean that ripgrep also uses PCRE2's replacement syntax. ripgrep uses the `interpolate` method on the `Captures` trait from the `grep-matcher` crate: https://docs.rs/grep-matcher/0.1.4/grep_matcher/trait.Captures.html#method.interpolate

The PCRE2 implementation of the `Captures` trait is here: https://github.com/BurntSushi/ripgrep/blob/f51b762c6da202247f609bf8494fb82b31f9c285/crates/pcre2/src/matcher.rs#L348-L356

It is intentionally missing the `interpolate` method so that all regex engines use the same replacement syntax. The reasoning for this is described in #1538. 

---
