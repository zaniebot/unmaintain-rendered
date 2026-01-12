```yaml
number: 2484
title: No match highlighting when using pcre2
type: issue
state: closed
author: fabian-thomas
labels:
  - invalid
assignees: []
created_at: 2023-04-03T20:22:52Z
updated_at: 2023-04-04T21:47:31Z
url: https://github.com/BurntSushi/ripgrep/issues/2484
synced_at: 2026-01-12T16:13:24Z
```

# No match highlighting when using pcre2

---

_@fabian-thomas_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Arch repositories

#### What operating system are you using ripgrep on?

Arch linux, all packages latest

#### Describe your bug.

When using the pcre2 regex engine, rg does not highlight matches.

#### What are the steps to reproduce the behavior?

`rg -p -P -e "^(?=.*one)"`

with a file with content:
```
one
```

I would assume this to highlight the matched word one, as rg would do without the `-P` flag.


---

_Comment by @BurntSushi on 2023-04-04 01:24_

Your regex only matches the empty string, so no highlighting is correct.

Look around only controls whether something matches. The whole point of it is that it doesn't change the offsets of the match. In other words, you're asking for something that is directly contrary to the feature you're using.

---

_Closed by @BurntSushi on 2023-04-04 01:24_

---

_Label `invalid` added by @BurntSushi on 2023-04-04 01:24_

---

_Comment by @fabian-thomas on 2023-04-04 06:01_

Thanks for the explanation! Makes sense. So there is absolutely no way to get what I want with one ripgrep call?
Note that the real pattern I want to search for is something like this: `rg -P -e "^(?=.*one)(?=.*three)"`. So basically match all lines that have both one and three, and I would want highlighting for all occurences of one and three. 

What I could do I guess, is to pipe the results through ripgrep one more time and then highlight all occurences of one and three. 


---

_Comment by @BurntSushi on 2023-04-04 11:36_

> What I could do I guess, is to pipe the results through ripgrep one more time and then highlight all occurences of one and three.

Yes. This is the way for now. There is also a ticket for adding more sophisticated boolean matching that would probably let you achieve your objective here. On mobile, or otherwise I would link it.

---

_Comment by @fabian-thomas on 2023-04-04 21:39_

For future reference:
#875 is the issue about boolean matching.
I'm just mentioning here that ugrep might be the tool you are searching if you arive at this issue. It supports matching against multiple patterns combined with an and. The command for that is `ugrep --and one --and three`.

---

_Comment by @BurntSushi on 2023-04-04 21:47_

`git grep` supports it too.

---
