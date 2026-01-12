```yaml
number: 2623
title: "-w/--word-regexp sometimes returns odd results"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
  - rollup
assignees: []
created_at: 2023-10-10T00:05:08Z
updated_at: 2023-10-10T00:29:55Z
url: https://github.com/BurntSushi/ripgrep/issues/2623
synced_at: 2026-01-12T16:13:24Z
```

# -w/--word-regexp sometimes returns odd results

---

_@BurntSushi_

It turns out that ripgrep's `-w/--word-regexp` flag doesn't quite do what it claims to. And as a result, it actually differs from GNU grep in some cases (and even with PCRE2 within ripgrep):

```
$ echo '###' | grep -w -o .
#
#
#
$ echo '###' | rg-13.0.0 -w -o .
#
#
$ echo '###' | rg-13.0.0 -P -w -o .
#
#
#
```

ripgrep 14 will fix this:

```
$ echo '###' | rg-14.0.0 -w -o .
#
#
#
```

The actual issue here is that ripgrep used a hacky work-around to implement `-w/--word-regexp` that just didn't work right in all cases. For PCRE2, it was implemented via look-around assertions that always got it right:

https://github.com/BurntSushi/ripgrep/blob/52731cdfb2e1f10ee068d8c40fac819484615c81/crates/pcre2/src/matcher.rs#L64-L69

The work-around I used basically tried to emulate look-around with capture groups. And while it works in a lot of cases, it doesn't work in all of them. I spent quite a bit of time trying to figure out how to fix the work-around once and for all, but couldn't see a way to make the code obviously correct. Instead, I just added support for `\b{start-half}` and `\b{end-half}` word boundary assertions that do exactly what the look-around does in PCRE2. See: https://github.com/rust-lang/regex/issues/469

---

_Label `bug` added by @BurntSushi on 2023-10-10 00:05_

---

_Label `rollup` added by @BurntSushi on 2023-10-10 00:05_

---

_Closed by @BurntSushi on 2023-10-10 00:29_

---
