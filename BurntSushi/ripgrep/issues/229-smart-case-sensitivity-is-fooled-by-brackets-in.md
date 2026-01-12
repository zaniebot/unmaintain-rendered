```yaml
number: 229
title: smart-case sensitivity is fooled by brackets in search pattern
type: issue
state: closed
author: ngirard
labels:
  - bug
  - question
assignees: []
created_at: 2016-11-09T22:12:47Z
updated_at: 2016-11-28T22:58:32Z
url: https://github.com/BurntSushi/ripgrep/issues/229
synced_at: 2026-01-12T18:23:11Z
```

# smart-case sensitivity is fooled by brackets in search pattern

---

_@ngirard_

As title says, the case sensivity is not respected any more when the search pattern contains capitals within brackets.
As an example,
`rg --smart-case '[EÉ]conomie'` 
returns also lowercase matches ("economie").

---

_Comment by @BurntSushi on 2016-11-09 22:58_

I see. Under what circumstances should an uppercase literal be detected? Would `\p{Lu}` qualify? What about `[[:upper:]]`? Or what about `[@-a]`, which contains `A-Z`?


---

_Label `bug` added by @BurntSushi on 2016-11-09 22:58_

---

_Label `question` added by @BurntSushi on 2016-11-09 22:58_

---

_Comment by @BurntSushi on 2016-11-15 23:24_

Some more thoughts on this.
1. There is an uppercase literal in the regex pattern itself. For example, `[A-Z]foo` contains an uppercase literal, and therefore the search would be case sensitive. Another example, `foo\p{Lu}` (where `\p{Lu}` is the set of all uppercase Unicode letters) contains no uppercase literals and therefore would be a case insensitive search.
2. There are no literal characters at all. For example, `\p{Lu}` contains no literals, and therefore should be a case sensitive search.

(2) is actually slightly tricky to implement because the regex parser is a bit too ambitious. e.g., After parsing, there's no actual way to distinguish `\p{Lu}` from its correspond character class as if it were manually typed by the user.

This is kind of why I hate the smart case feature and why it's not the default. In the simple case, it has nice behavior, but when your regex grows beyond literals, its behavior becomes unclear and less intuitive.

In the absence of input from others, I think I'd like to just implement rule (1) and call it a day. It would, for example, work in @ngirard's case.


---

_Closed by @BurntSushi on 2016-11-28 22:58_

---
