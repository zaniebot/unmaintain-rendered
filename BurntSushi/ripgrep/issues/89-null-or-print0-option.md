```yaml
number: 89
title: "\"--null\" or \"--print0\" option?"
type: issue
state: closed
author: gwerbin
labels:
  - enhancement
assignees: []
created_at: 2016-09-25T20:02:07Z
updated_at: 2016-09-26T23:21:33Z
url: https://github.com/BurntSushi/ripgrep/issues/89
synced_at: 2026-01-12T18:23:11Z
```

# "--null" or "--print0" option?

---

_@gwerbin_

Is there any chance you'd be interested in implementing a feature like Ag's `--null` option, and Ack's (not to mention find's) `--print0` option? If you did, it'd be really easy to make [tag](http://github.com/aykamko/tag/) work with Ripgrep, not to mention more supportive of machine-parsing in general.


---

_Comment by @BurntSushi on 2016-09-25 20:14_

`grep` has this too. Can we write the specification first? Here's my first attempt:

```
--null
    Whenever a file name is printed, follow it with a NUL byte.
    This includes printing filenames before matches, and when printing
    a list of matching files such as with --count, --files-with-matches
    and --files.
```


---

_Comment by @kaushalmodi on 2016-09-25 20:48_

I would like to have this option too..

For now, my woraround is to do..

```
rg foo | tr '\n' '\0'
```

But hopefully internal support of null-separated output makes this faster.

My actual use case is

```
rg -g '' --files | tr '\n' '\0' >! proj_file_list.txt
```


---

_Comment by @BurntSushi on 2016-09-25 20:52_

@kaushalmodi Unrelated: your `-g ''` is I think completely superfluous.


---

_Comment by @gwerbin on 2016-09-25 21:44_

@BurntSushi LGreatTM, that's a more detailed spec than any of the other tools mentioned here. But maybe it should be both `--print0` and `--null`. Ag has them aliased to each other, Ack only supports `--print0`, and Pt and Git Grep _only_ support `--null`.


---

_Comment by @kaushalmodi on 2016-09-25 21:58_

@BurntSushi 

> Unrelated: your -g '' is I think completely superfluous.

Thanks for letting me know.. I'll switch to `rg --files` then.

@gwerbin 

There's `-0` too. So `-0` or `--null` or `--print0`.


---

_Comment by @BurntSushi on 2016-09-25 22:06_

In general, when there's been variation in flag options, I've tried to go with what grep does. I'm not a big fan of lots of aliases for doing the same thing, although I have done it in a few places. For a more niche option like this, I'd rather not increase the number of flags.


---

_Comment by @gwerbin on 2016-09-25 22:08_

@BurntSushi good to be consistent. Unfortunately I have absolutely no experience programming in Rust, otherwise I'd just make a PR for proof-of-concept.

@kaushalmodi I avoided `-0` because I always thought of that as an _input_ parsing option as per, e.g. xargs.


---

_Label `enhancement` added by @BurntSushi on 2016-09-25 22:34_

---

_Comment by @emlyn on 2016-09-26 08:59_

Just a nit about the wording of the spec (`Whenever a file name is printed, proceed it with a NUL byte`). I've not heard "proceed" used in this way before, is it a misspelling of "precede", or does it mean "follow"?


---

_Comment by @BurntSushi on 2016-09-26 11:29_

@emlyn "to proceed" is "to come after," so yes, "follow" is right. :-)

Looking at the dictionary, it does feel a little non-standard to me. The flavor isn't quite there. "follow" is probably a better word. I fixed the wording. Thanks!


---

_Comment by @gwerbin on 2016-09-26 13:11_

@emlyn Good catch.  I definitely thought he meant "precede", which in hindsight wouldn't make sense.

@BurntSushi "proceed" is a motion/action verb, meaning either "to continue" or to physically "go forward."


---

_Closed by @BurntSushi on 2016-09-26 23:21_

---

_Comment by @BurntSushi on 2016-09-26 23:21_

All set! Will be in the next release, hopefully tonight.


---
