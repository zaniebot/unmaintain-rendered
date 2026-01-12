```yaml
number: 1938
title: support POSIX grep (and maybe GNU grep) user interface
type: issue
state: closed
author: xenoterracide
labels:
  - wontfix
assignees: []
created_at: 2021-07-17T00:33:33Z
updated_at: 2021-07-17T02:20:15Z
url: https://github.com/BurntSushi/ripgrep/issues/1938
synced_at: 2026-01-12T16:13:24Z
```

# support POSIX grep (and maybe GNU grep) user interface

---

_@xenoterracide_

> I have a dream! of a GNU-less linux distribution, with no FSF software required (at runtime).

-- me since '05

ok, what I'd like to see, is if the 0th argument, which should be the program's invocation name is `grep` (or the others? `igrep`) 
that it conforms to POSIX as far as the required options. https://pubs.opengroup.org/onlinepubs/9699919799/utilities/grep.html it would be eventually nice to support the "proprietary" GNU extensions as well.

P.S. I actually kind of like rg better in some ways, but for a full replacement...

---

_Comment by @BurntSushi on 2021-07-17 00:54_

I think this FAQ item is relevant: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#posix4ever

More generally, the way you've worded your feature request doesn't quite give me confidence that you fully understand what it would take to make a POSIX compliant grep. :-) Merely adding support for "required options" is nowhere near enough. If it were really that simple, I would have absolutely made that available by now. The truly "difficult" aspects of POSIX support are in the regex engine itself. In order to make that work, you really need to set out---at the design level---to build the regex engine with POSIX support in mind.

Nevermind having to support locales...

Also, I've never heard of `igrep`.

I'm happy for you (or anyone) to use this issue for Q&A if you're curious about how you might go about building such a tool, but since ripgrep will never be that tool, I'm going to close this.

---

_Closed by @BurntSushi on 2021-07-17 00:54_

---

_Label `wontfix` added by @BurntSushi on 2021-07-17 00:54_

---

_Comment by @BurntSushi on 2021-07-17 00:55_

> I have a dream! of a GNU-less linux distribution, with no FSF software required (at runtime).

You might want to look into things like BSD grep or even busybox.

---

_Comment by @xenoterracide on 2021-07-17 02:20_

`igrep` and others are oddly just like `grep -i` I honestly have no idea why they exist

---
