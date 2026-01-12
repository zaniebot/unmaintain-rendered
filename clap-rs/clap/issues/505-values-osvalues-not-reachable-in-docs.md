```yaml
number: 505
title: Values, OsValues not reachable in docs
type: issue
state: closed
author: bluss
labels:
  - C-enhancement
  - A-docs
assignees: []
created_at: 2016-05-13T12:57:58Z
updated_at: 2018-08-02T03:29:49Z
url: https://github.com/clap-rs/clap/issues/505
synced_at: 2026-01-12T16:14:09Z
```

# Values, OsValues not reachable in docs

---

_@bluss_

Iterators Values and OsValues are not reachable in the docs (they are black, non-links). This is because they are `pub` but not really reachable, so they are not linked.

Suggestion: Either make them reachable, or expand the documentation for values_of, values_of_os to spell it out exactly what the return value is (Iterator of X).

I'm new to this library, but it seems very solid, congratulations :smile:


---

_Comment by @kbknapp on 2016-05-13 14:38_

Thanks for pointing this out! Yeah, it's been in the back of my mind that I need to go through and not only ensure each of these types of things is reachable, but also inter-linked between all the different pieces.

I'm almost done with the base of the 3.x work to allow non-stringly typed usage (i.e. using enums as arg and subcommand keys), but I'll see if I can do this on the 2.x branch too.

Thanks for the kind words :smile:


---

_Label `T: enhancement` added by @kbknapp on 2016-05-13 14:38_

---

_Label `P2: need to have` added by @kbknapp on 2016-05-13 14:38_

---

_Label `C: docs` added by @kbknapp on 2016-05-13 14:38_

---

_Label `D: easy` added by @kbknapp on 2016-05-13 14:38_

---

_Label `E: tedious` added by @kbknapp on 2016-05-13 14:38_

---

_Label `W: 2.x` added by @kbknapp on 2016-05-13 14:38_

---

_Label `W: 3.x` added by @kbknapp on 2016-05-13 14:38_

---

_Comment by @kbknapp on 2016-05-13 14:39_

I'll use this as the tracking issue for:
- [x] Ensure all public facing types are reachable
  - [x] `Values`
  - [x] `OsValues`
  - [x] `ArgSettings`
- [x] Ensure all types are inter-linked


---

_Comment by @bluss on 2016-05-13 17:13_

ArgSettings is another.


---

_Comment by @kbknapp on 2016-05-13 17:36_

ArgSettings was intentionally hidden since it's used internally, but I haven't yet decided if I wanted to expose it publicly.


---

_Comment by @bluss on 2016-05-13 18:22_

it's used as a public argument type for a method on Arg I believe. Since it's not linked, it's hard to understand exactly how those methods are used.


---

_Comment by @kbknapp on 2016-05-13 21:40_

Ah you're right, my mistake!


---

_Referenced in [clap-rs/clap#507](../../clap-rs/clap/pulls/507.md) on 2016-05-13 22:54_

---

_Referenced in [clap-rs/clap#508](../../clap-rs/clap/pulls/508.md) on 2016-05-15 18:24_

---

_Comment by @kbknapp on 2016-05-15 18:31_

#508  takes care of all this. Finally, all types are inter-linked and the docs feel much more usable.


---

_Closed by @homu on 2016-05-15 19:32_

---
