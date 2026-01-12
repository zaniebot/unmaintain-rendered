```yaml
number: 1025
title: allow handling lookbehind assertions of non-fixed length
type: issue
state: closed
author: timotheecour
labels:
  - question
assignees: []
created_at: 2018-08-23T13:20:53Z
updated_at: 2018-08-23T13:31:02Z
url: https://github.com/BurntSushi/ripgrep/issues/1025
synced_at: 2026-01-12T16:13:22Z
```

# allow handling lookbehind assertions of non-fixed length

---

_@timotheecour_

using this pattern `(?<=(foo|mybar)mypattern` results in:
`error compiling pattern at offset 0: lookbehind assertion is not fixed length`
this severely limits usability of lookbehind assertions
is that a limitation with rust's version of pcre2 ?
it doesn't look like a fundamental limitation: other languages/libraries handle lookbehind assertions of non-fixed length just fine (eg https://dlang.org/phobos/std_regex.html and IIRC, also https://nim-lang.org/docs/re.html)



---

_Comment by @okdana on 2018-08-23 13:27_

It's a limitation of PCRE itself

https://www.pcre.org/current/doc/html/pcre2pattern.html#SEC20

---

_Comment by @BurntSushi on 2018-08-23 13:29_

> is that a limitation with rust's version of pcre2?

There is no "Rust" version PCRE2. This is a limitation of PCRE2 itself, as you can see, the same error occurs with `grep -P` (using PCRE1) and `pcre2grep` (using PCRE2):

```
$ grep -P '(?<=(foo|mybar))mypattern' -r
grep: lookbehind assertion is not fixed length
$ pcre2grep '(?<=(foo|mybar))mypattern' -r
pcre2grep: Error in command-line regex at offset 0: lookbehind assertion is not fixed length
```

If you [consult the PCRE2 documentation](https://www.pcre.org/current/doc/html/pcre2pattern.html#lookbehind), you'll see that you can actually rewrite your pattern without the superfluous parentheses, and it will work:

```
$ rg -P '(?<=foo|mybar)mypattern'
```

You have a few choices here:

1. Lobby PCRE2 to lift this restriction. If they lift it, then you can use it inside ripgrep.
2. Find a different code search tool that supports your desired semantics. (I am not aware of one.)
3. Maintain a fork of ripgrep and add support for an additional regex engine that supports the semantics you want.

This is classic example of how adding features has a snowball effect and generates more maintenance burden. At some point, I have to say No. Also, I don't buy your claim of "severely limits usability of lookbehind assertions."

---

_Closed by @BurntSushi on 2018-08-23 13:29_

---

_Label `question` added by @BurntSushi on 2018-08-23 13:29_

---
