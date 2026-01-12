```yaml
number: 180
title: A readline flag
type: issue
state: closed
author: ticki
labels:
  - question
assignees: []
created_at: 2016-10-14T17:14:18Z
updated_at: 2016-11-18T01:49:36Z
url: https://github.com/BurntSushi/ripgrep/issues/180
synced_at: 2026-01-12T18:23:11Z
```

# A readline flag

---

_@ticki_

Regular expressions do not work well together with shells, due to escapes, canonicalization, and parsing. I suggest that a flag for reading a line from the user (`-r` or `-R`) and using this as the regex, effectively bypassing the shell weirdness.

This could possibly even be the behavior when no argument is given.


---

_Comment by @kaushalmodi on 2016-10-14 17:20_

Probably related to this? https://github.com/BurntSushi/ripgrep/issues/102

If so, this (https://github.com/BurntSushi/ripgrep/issues/102#issuecomment-249620557) is the solution.


---

_Comment by @BurntSushi on 2016-10-14 17:39_

Could you please provide a use case that motivates this feature? What does this provide that single quotes or even `$'...'` don't?


---

_Comment by @ticki on 2016-10-14 17:41_

It's faster to type.


---

_Comment by @BurntSushi on 2016-10-14 17:45_

@ticki I'd like some examples please. I'm particularly interested in examples that are common.

Are there other tools that implement this particular feature? I can't recall seeing it before.


---

_Comment by @BurntSushi on 2016-10-14 17:57_

Another problem I kind of have with this feature is that it breaks shell history. When I run a `grep` or an `rg` command, I often need to run it multiple times because I need to tweak my pattern or change some other setting. If I have to re-type the regex every time, I'm going to get annoyed pretty quickly.

I do agree that handling escapes is annoying, although I'm not sure how often it comes up in practice. I'd appreciate some brainstorming with other ways to fix this. One idea is #7, although committing the pattern to a file is a bit heavyweight just to avoid escapes, but it does not suffer from the shell history problem (because I can just edit the file).


---

_Label `question` added by @BurntSushi on 2016-10-14 17:57_

---

_Comment by @BurntSushi on 2016-10-15 13:54_

I was thinking about this last night before bed. So I guess it doesn't necessarily have to break shell history. If you did:

```
$ rg --readline hi <<EOF
\w+
EOF
```

Then scrolling back to this command will preserve the heredoc, which I think satisfies at least one important objection to this feature. It does however mean that you can't search `stdin` with this feature enabled, which kind of seems like another stumbling block to me.


---

_Comment by @zokier on 2016-10-23 14:59_

If #7 is resolved then you could handle `-f-` as "read patterns from stdin", which is what GNU grep does. In grep the interaction with searching stdin seems buggy/weird, but I don't think that will be much of a problem in practice.


---

_Comment by @BurntSushi on 2016-11-06 17:27_

OK, I'm closing this in favor of doing #7 with support for stdin.


---

_Closed by @BurntSushi on 2016-11-06 17:27_

---

_Comment by @BurntSushi on 2016-11-18 01:49_

@ticki FYI, this is done. This works now (master, will be in next `0.3` release), for example:

```
$ rg -f-
\p{Lu}\pL!
^D
```

heredocs work too:

```
$ rg -f- <<EOF
> \p{Lu}\pL!
> EOF
```


---
