```yaml
number: 729
title: Handling of regex patterns that can match the empty string
type: issue
state: closed
author: simark
labels:
  - question
  - wontfix
assignees: []
created_at: 2018-01-03T20:58:36Z
updated_at: 2018-01-03T22:00:45Z
url: https://github.com/BurntSushi/ripgrep/issues/729
synced_at: 2026-01-12T16:13:22Z
```

# Handling of regex patterns that can match the empty string

---

_@simark_

A regex that can match an empty string, like this:

    rg '(thisstringdoesnotexist)?'

makes rg output one match for every line of every file searched.  With the --vimgrep option, it's one match per character.  When searching a big directory, it makes rg take a lot of time and resources to output meaningless results.  I think that rg should not output matches that are zero-length, I can't think of a use case where it would be useful.

---

_Comment by @okdana on 2018-01-03 21:06_

There is one use case i know of: If you want to colourise matches, but don't want to restrict the output to the lines containing those matches, it's common (with `grep` too) to do something like `rg '^|foo'`. That way every instance of `foo` is coloured, but it prints all of the 'non-matching' lines too

I suppose `rg -C65535 foo` would be a viable alternative to that though

Edit: In your case it just seems like a weird pattern though. Why would you search for `(thisstringdoesnotexist)?` instead of just `thisstringdoesnotexist`?

---

_Comment by @simark on 2018-01-03 21:15_

Ah ok thanks, didn't know about that use case...

---

_Comment by @simark on 2018-01-03 21:19_

> Edit: In your case it just seems like a weird pattern though. Why would you search for (thisstringdoesnotexist)? instead of just thisstringdoesnotexist?

Indeed, it would not be a useful search pattern.  However, I am integrating rg in an IDE, and am trying to handle gracefully anything the user throws at us, good or bad.

---

_Comment by @BurntSushi on 2018-01-03 22:00_

This is intended behavior. You can simply never avoid resource hungry regexes. For example, `rg .` is a thing that will also match and color every character. You'll need to design around it.

---

_Closed by @BurntSushi on 2018-01-03 22:00_

---

_Label `wontfix` added by @BurntSushi on 2018-01-03 22:00_

---

_Label `question` added by @BurntSushi on 2018-01-03 22:00_

---
