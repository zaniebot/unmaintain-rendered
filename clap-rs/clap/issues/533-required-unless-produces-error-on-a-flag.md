---
number: 533
title: required_unless produces error on a flag
type: issue
state: closed
author: joshtriplett
labels:
  - C-enhancement
  - A-docs
  - A-parsing
assignees: []
created_at: 2016-06-22T02:46:13Z
updated_at: 2016-06-24T00:33:36Z
url: https://github.com/clap-rs/clap/issues/533
synced_at: 2026-01-10T01:26:31Z
---

# required_unless produces error on a flag

---

_Issue opened by @joshtriplett on 2016-06-22 02:46_

As the clap documentation notes, you can't make a flag required.  However, it should be possible to mark a flag as `.required_unless` another flag or argument.  Doing so currently produces a runtime panic.

You can work around this with another argument by marking the argument as `.required_unless` the flag instead. However, given two flags you can't.

(Alternatively, if this would make the require requirements more difficult to express, the documentation could instead explicitly note this case and point to another way to say "one or more of these are required".)


---

_Comment by @kbknapp on 2016-06-23 03:43_

The thought process behind that is somewhat opinionated...and being that `clap` is becoming more heavily used I'm willing to discuss changing.

---

Currently flags can't be required because they're simply boolean values. If one should be required, then I would have thought the developer should "imply" it's use (perhaps with a note in the usage stating that it's implied in certain cases). IMO if flag `--foo` is required, then forcing the end user to type `--foo` unnecessary verbosity. There isn't any additional context passed to the developer/program by forcing them to do so.

The case against my thinking would be forcing the user to type `--foo` means they _know_ that particular functionality/feature is being used.

I'm curious to know what others thoughts on the matter are?

---

If the above reasoning is correct, and there is a case where `required_unless` falls within that reasoning yet still causes a runtime panic...that's a bug. In which case I just need some clarity on this particular case :smile:


---

_Label `T: RFC / question` added by @kbknapp on 2016-06-23 03:46_

---

_Comment by @joshtriplett on 2016-06-23 04:31_

I completely agree with the rationale for not allowing `.required` on a flag; please don't change that.

Here's my use case: I have a command similar to `git rebase`, which takes a flag `--interactive` (`-i`) and a positional argument `onto`.  You can run `rebase -i` with no other arguments, or `rebase some_commit`, or `rebase -i some_commit`, but `rebase` without either `-i` or `onto` makes no sense as it would have nothing to do.  So, I'd like to say "you must specify at least one of `onto` or `--interactive`.

I'd be happy with another way of doing this, such as a `require_any` method on `ArgGroup`, especially if that would give clap enough semantic information to give an error message like "you must specify at least one of --interactive/-i or base" (along with the help).  Ideally the usage message will also reflect this by calling out the flags in that `ArgGroup` and not burying them in `[FLAGS]`.


---

_Comment by @kbknapp on 2016-06-23 13:33_

Ah, ok I understand, thanks! I think what I'll try to do is add a method `ArgGroup::multiple(bool)` which when combined with `ArgGroup::required(true)` does exactly like you're saying. Vice the normal `ArgGroup::required(true)` which makes one of them required, but _only_ one and the rest are all mutually exclusive.


---

_Label `T: enhancement` added by @kbknapp on 2016-06-23 13:34_

---

_Label `C: docs` added by @kbknapp on 2016-06-23 13:34_

---

_Label `D: intermediate` added by @kbknapp on 2016-06-23 13:34_

---

_Label `P3: want to have` added by @kbknapp on 2016-06-23 13:34_

---

_Label `C: parsing` added by @kbknapp on 2016-06-23 13:34_

---

_Label `W: 2.x` added by @kbknapp on 2016-06-23 13:34_

---

_Label `C: arg groups` added by @kbknapp on 2016-06-23 13:34_

---

_Label `T: RFC / question` removed by @kbknapp on 2016-06-23 13:34_

---

_Added to milestone `2.7.0` by @kbknapp on 2016-06-23 13:34_

---

_Assigned to @kbknapp by @kbknapp on 2016-06-23 13:34_

---

_Comment by @kbknapp on 2016-06-23 15:29_

This is implemented with #535 


---

_Closed by @homu on 2016-06-23 20:26_

---

_Comment by @joshtriplett on 2016-06-23 20:42_

@kbknapp Thanks for the fast fix!  I'll test this today, including what the error message looks like for not providing one of the arguments.

Any plans for a clap release incorporating this?  (I can pull clap from git for testing, but it'd be nice to have a released version on crates.io.)


---

_Comment by @joshtriplett on 2016-06-23 21:36_

@kbknapp Just tested this; works perfectly.  Thanks!


---

_Referenced in [clap-rs/clap#537](../../clap-rs/clap/issues/537.md) on 2016-06-23 21:43_

---

_Comment by @kbknapp on 2016-06-24 00:19_

@joshtriplett Once #376 #536 are merged I'll post the new version to crates.io. If #376 takes too much time I may just bump and publish prior. So in the next day or so I should have the new version out on crates.io


---

_Comment by @kbknapp on 2016-06-24 00:33_

Add #539 #538 and #537 to that list :stuck_out_tongue_winking_eye: 


---
