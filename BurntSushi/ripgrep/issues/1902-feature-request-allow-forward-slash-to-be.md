```yaml
number: 1902
title: "[Feature request] Allow forward slash (/) to be optionally escaped in patterns"
type: issue
state: closed
author: comex
labels:
  - enhancement
assignees: []
created_at: 2021-06-17T22:09:27Z
updated_at: 2021-06-17T22:26:54Z
url: https://github.com/BurntSushi/ripgrep/issues/1902
synced_at: 2026-01-12T16:13:24Z
```

# [Feature request] Allow forward slash (/) to be optionally escaped in patterns

---

_@comex_

#### Describe your feature request

Many languages use `/` to indicate the start and end of a regular expression, and thus require any forward slashes within the pattern to be escaped as `\/`.

ripgrep does not need to explicitly delimit regexes, and does not treat `/` as special.  But sometimes habit leads me to mistakenly type `\/` anyway.  This produces an error:
```
regex parse error:
    \/foo
    ^^
error: unrecognized escape sequence
```

Fair enough.  But given ripgrep's status as an interactive tool, it would be nice if it would just Do What I Mean, perhaps warning about the unnecessary escape but then proceeding, rather than making me go back and edit the command.

I tested various other regular expression APIs and tools that don't use `/` as a delimiter: Python `re`, PCRE, GNU grep, and BSD grep on macOS.  All of these accepted `\/` (with no warning) and treated it as `/`.  (Most of those seemed to allow unnecessary backslash escapes of arbitrary characters; Python allowed it for characters other than letters and numbers.  Personally I'd prefer a more conservative approach that only allows it for `/`.)

---

_Comment by @BurntSushi on 2021-06-17 22:25_

This is better tracked here: https://github.com/rust-lang/regex/issues/501

While I suppose ripgrep could allow this independently of what the regex engine does, it would probably be more trouble than it's worth.

---

_Label `enhancement` added by @BurntSushi on 2021-06-17 22:25_

---

_Comment by @comex on 2021-06-17 22:26_

Aha.

---

_Closed by @comex on 2021-06-17 22:26_

---
