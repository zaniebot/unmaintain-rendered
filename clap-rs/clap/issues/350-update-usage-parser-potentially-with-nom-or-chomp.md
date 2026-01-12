```yaml
number: 350
title: Update usage parser (potentially with nom or chomp)
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-parsing
assignees: []
created_at: 2015-11-24T17:28:41Z
updated_at: 2018-08-02T03:29:46Z
url: https://github.com/clap-rs/clap/issues/350
synced_at: 2026-01-12T16:14:09Z
```

# Update usage parser (potentially with nom or chomp)

---

_@kbknapp_

With [nom](https://github.com/Geal/nom) recently reaching 1.0 I'm wondering if using would be any benefit to this project. Particularly in the test-ability, performance, and maintainability.

Because I'm very unfamiliar with it, but slowly reading through it, this may be slow going.

Perhaps initially evaluating on the [from_usage parser](https://github.com/kbknapp/clap-rs/blob/41bd23300de7e84d288b161073079e4fb9d49373/src/usageparser.rs).

To be clear, in order to be merged into mainline it needs to **not** impact performance at all (which I don't believe will be an issue), and must improve at least one of the items above.


---

_Label `T: enhancement` added by @kbknapp on 2015-11-24 17:28_

---

_Label `P4: nice to have` added by @kbknapp on 2015-11-24 17:28_

---

_Label `D: intermediate` added by @kbknapp on 2015-11-24 17:28_

---

_Label `C: parsing` added by @kbknapp on 2015-11-24 17:28_

---

_Label `W: 1.x` added by @kbknapp on 2015-11-24 17:28_

---

_Comment by @Vinatorul on 2015-11-25 07:59_

From the one side - we can rewrite parser no `nom` and make it more clear.
From the other - `nom` is new dependency.

As for me - I'm pretty much for rewriting :)


---

_Comment by @kbknapp on 2015-11-25 08:15_

Yeah I understand the new dep thing, but if we can make the code more readable, concise, testable, and performant then I'm all ok using a new dep. If we can do all the above by just re-writing the parser I'm good with that too. But I would imagine handwriting a new parser might be re-implenting things already solved in`nom`

Also `chomp` looks promising too but the fact that it's less established and pre-1.0 means I'd lean towards `nom` all things being equal.

To be clear, I'm not really advocating using `nom` in master yet, more just testing to see if it's something we should start using.


---

_Comment by @kbknapp on 2015-11-25 08:19_

In all honesty using an iterator in the usage parser is silly, but I guess it made sense to me when I did it. We'd probably get way better performance using a real parser, which is what I'd really like to explore. Whether that's using something like `nom` or home rolled? Let's try them both and see what looks/performs best! :smile:


---

_Added to milestone `v2.0` by @kbknapp on 2016-01-23 15:51_

---

_Label `W: 2.x` added by @kbknapp on 2016-01-25 18:27_

---

_Label `W: 1.x` removed by @kbknapp on 2016-01-25 18:27_

---

_Renamed from "Evaluate potential for use of nom" to "Update usage parser (potentially with nom or chomp)" by @kbknapp on 2016-01-25 18:27_

---

_Comment by @kbknapp on 2016-01-26 08:23_

Closed with #386 


---

_Closed by @kbknapp on 2016-01-26 08:23_

---
