```yaml
number: 1055
title: "Make `textwrap` an optional dependency"
type: issue
state: closed
author: H2CO3
labels:
  - C-enhancement
  - A-help
  - E-medium
assignees: []
created_at: 2017-09-27T17:40:44Z
updated_at: 2018-08-02T03:30:12Z
url: https://github.com/clap-rs/clap/issues/1055
synced_at: 2026-01-12T16:14:10Z
```

# Make `textwrap` an optional dependency

---

_@H2CO3_

It would be nice to make the `textwrap` crate an optional dependency, so that those who don't care that much about the nice formatting wouldn't need to install the transitive closure of the dependencies of `textwrap` (most notably, `libc` and `winapi`).

This would also cut down on the quantity of `unsafe` used (again, transitively) by the dependent crates, should they choose to opt out of pretty help text wrapping.


---

_Comment by @kbknapp on 2017-10-04 03:04_

I'd be fine with this, if someone wants an easy PR.

:+1: 

---

_Label `C: help message` added by @kbknapp on 2017-10-04 03:04_

---

_Label `D: easy` added by @kbknapp on 2017-10-04 03:04_

---

_Label `M: mentored` added by @kbknapp on 2017-10-04 03:04_

---

_Label `P4: nice to have` added by @kbknapp on 2017-10-04 03:04_

---

_Label `T: enhancement` added by @kbknapp on 2017-10-04 03:04_

---

_Label `W: 2.x` added by @kbknapp on 2017-10-04 03:04_

---

_Comment by @H2CO3 on 2017-10-04 09:22_

Sure thing, I'll make the PR for this one as well.


---

_Comment by @H2CO3 on 2017-10-05 08:15_

Looks like we'll need to discuss this one a bit more. Here's the complete problem scenario:

* The doc says that with feature`wrap_help` enabled, it will wrap the text respecting the actual with of the terminal rather than assuming 120 columns. It also says that this feature will build the `term_size` crate as a dependency.
* This would make me assume that `term_size` is not necessary when this feature is disabled. However, `textwrap` is a non-optional dependency, and it has `term_size` as a non-optional dependency itself. This means that `term_size` is always a dependency.
* Does this mean that `textwrap` should also make `term_size` an optional dependency; better yet, a non-dependency? As far as I can tell, that crate does not internally rely on knowing the terminal width; it merely provides a convenience callthrough/adaptor interface for `term_size`. I think that it is poor code organization to hard depend on a crate that is not used directly, only for the sake of providing a convenience adaptor for it.
* Consequently, I think I'll have to go to the `textwrap` crate's author and ask him/her to remove `term_size` as a dependency before I can continue implementing this PR.


---

_Referenced in [mgeisler/textwrap#101](../../mgeisler/textwrap/issues/101.md) on 2017-10-05 09:13_

---

_Comment by @H2CO3 on 2017-10-05 18:46_

The issue referred above has been resolved lightning fast. 👍 Once the next release of `textwrap` is out, I'll write up the necessary changes as a pull request.


---

_Comment by @mgeisler on 2017-10-05 20:56_

Textwrap 0.9.0 has been released with this change, have fun! :-)

---

_Referenced in [clap-rs/clap#1059](../../clap-rs/clap/pulls/1059.md) on 2017-10-06 11:18_

---

_Comment by @kbknapp on 2017-10-07 16:57_

Thanks @mgeisler for being flexible and helping with this!

---

_Closed by @kbknapp on 2017-10-07 16:57_

---

_Comment by @H2CO3 on 2017-10-07 19:02_

@kbknapp @mgeisler Thanks for your responsiveness and collaboration!


---

_Comment by @mgeisler on 2017-10-08 10:21_

Happy to help! I want my little crate to be useful in as many scenarios as possible.

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-11-13 01:59_

---
