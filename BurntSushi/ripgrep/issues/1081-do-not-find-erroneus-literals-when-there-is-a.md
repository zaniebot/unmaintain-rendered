```yaml
number: 1081
title: Do not find erroneus literals when there is a capture group and a character class of five or more
type: issue
state: closed
author: PeterSP
labels:
  - duplicate
assignees: []
created_at: 2018-10-08T19:12:05Z
updated_at: 2018-10-08T20:12:52Z
url: https://github.com/BurntSushi/ripgrep/issues/1081
synced_at: 2026-01-12T16:13:22Z
```

# Do not find erroneus literals when there is a capture group and a character class of five or more

---

_@PeterSP_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

I used `cargo install`

#### What operating system are you using ripgrep on?

```Darwin C02TM0LHG8WL 17.7.0 Darwin Kernel Version 17.7.0: Thu Jun 21 22:53:14 PDT 2018; root:xnu-4570.71.2~1/RELEASE_X86_64 x86_64```

#### If this is a bug, what is the actual behavior?

```
$ echo " a_" | rg ' ([abcde]+_)' --debug
DEBUG|grep_regex::literal|${HOME}/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-regex-0.1.1/src/literal.rs:110: required literal found: " _"
DEBUG|globset|${HOME}/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.2/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
$ echo " a_" | rg ' ([abcd]+_)' --debug
DEBUG|grep_regex::literal|${HOME}/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-regex-0.1.1/src/literal.rs:100: required literals found: [Cut( a), Cut( b), Cut( c), Cut( d), Complete( _)]
DEBUG|globset|${HOME}/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.2/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
 a_
```
Or, without all that noise:
```
$ echo " a_" | rg ' ([abcde]+_)'
$ echo " a_" | rg ' ([abcd]+_)'
 a_
```

#### If this is a bug, what is the expected behavior?

What do you think ripgrep should have done?

1. Not find `" _"` as a literal which neither matches all nor any of the strings the pattern desired matches.
2. Find `" a_"` as a match.

---

_Comment by @lespea on 2018-10-08 19:20_

Looks like the same issues as https://github.com/BurntSushi/ripgrep/issues/1081.  I tested the version of ripgrep with this fixed and it appears to work as expected.

    $  echo " a_" | rg ' ([abcde]+_)'; echo " a_" | rg ' ([abcd]+_)'
    a_
    a_

---

_Comment by @PeterSP on 2018-10-08 19:23_

It appears that this is a dupe of #1064 (in that it's fixed by the associated change).

```
$ cargo uninstall ripgrep
$ cargo install ripgrep --git git@github.com:BurntSushi/ripgrep.git
...
$ echo "abg" | rg 'a([bcdef]+g)' --debug
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:110: required literal found: "a"
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
abg
```

---

_Closed by @PeterSP on 2018-10-08 19:24_

---

_Comment by @PeterSP on 2018-10-08 19:58_

I can still add a regression test for this, but I suspect it's not worth it (as it's nearly identical to the regression test for #1064).

---

_Comment by @BurntSushi on 2018-10-08 20:12_

Yeah, I think we are all set here. Thanks for the report!

---

_Label `duplicate` added by @BurntSushi on 2018-10-08 20:12_

---
