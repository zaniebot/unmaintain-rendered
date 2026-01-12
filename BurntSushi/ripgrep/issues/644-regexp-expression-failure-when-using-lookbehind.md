```yaml
number: 644
title: Regexp expression failure when using lookbehind
type: issue
state: closed
author: scottchiefbaker
labels: []
assignees: []
created_at: 2017-10-20T17:22:19Z
updated_at: 2017-10-20T17:28:11Z
url: https://github.com/BurntSushi/ripgrep/issues/644
synced_at: 2026-01-12T16:13:22Z
```

# Regexp expression failure when using lookbehind

---

_@scottchiefbaker_

I was attempting to find all the lines in my code that contain foobar, and were **not** commented out. The way I've done this with `ack` and `ag` in the past has been using lookbehinds:

```
ack '(?<!#)foobar'
ag '(?<!#)foobar'
```

Attemping the same regexp in `rg` I get a parse error:

```
:rg '(?<!#)foobar'
Error parsing regex near '(?<!#)f' at character offset 2: Unrecognized flag: '<'. (Allowed flags: i, m, s, U, u, x.)
```

---

_Comment by @BurntSushi on 2017-10-20 17:27_

From the [README](https://github.com/BurntSushi/ripgrep#why-shouldnt-i-use-ripgrep):

> **ripgrep uses a regex engine based on finite automata, so if you want fancy regex features such as backreferences or look around, ripgrep won't give them to you.** ripgrep does support lots of things though, including, but not limited to: lazy quantification (e.g., a+?), repetitions (e.g., a{2,5}), begin/end assertions (e.g., ^\w+$), word boundaries (e.g., \bfoo\b), and support for Unicode categories (e.g., \p{Sc} to match currency symbols or \p{Lu} to match any uppercase letter). (Fancier regexes will never be supported.)

From the output of `rg --help`:

> ripgrep's regex engine uses finite automata and guarantees linear time
searching. Because of this, features like backreferences and arbitrary
lookaround are not supported.

---

_Closed by @BurntSushi on 2017-10-20 17:27_

---

_Comment by @BurntSushi on 2017-10-20 17:28_

You might consider `rg foobar | rg -v '^\s*#'` (or similar) instead.

---
