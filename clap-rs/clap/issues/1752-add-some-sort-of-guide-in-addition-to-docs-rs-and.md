---
number: 1752
title: Add some sort of Guide in addition to docs.rs and examples
type: issue
state: closed
author: CreepySkeleton
labels:
  - A-docs
assignees: []
created_at: 2020-03-18T00:29:45Z
updated_at: 2021-12-08T21:01:30Z
url: https://github.com/clap-rs/clap/issues/1752
synced_at: 2026-01-10T01:27:04Z
---

# Add some sort of Guide in addition to docs.rs and examples

---

_Issue opened by @CreepySkeleton on 2020-03-18 00:29_

### What's wrong

It's been consistently being reported that our documentation is hard to navigate. Many people come to the bugtracker to ask for a feature that [is already implemented](https://github.com/clap-rs/clap/issues/1738) or [can be achieved by using a set of related features](https://github.com/clap-rs/clap/issues/1751). 

It is apparent to me that our docs talk about "what" only, but they also should tell "how" and "why". The docs.rs is mostly about *possibilities*, but what people (especially beginners) seek for is *applications* and *common usages*. 

Examples help, but they can only cover so much. Also, examples show "how", but they can hardly explain "why". Sometimes it's hard to generalize a specific example further.

I would like to stress the "beginners" part here. The first thing many Rust newbies that come from other languages look for is "command line parser". Obviously, they get directed here (also to `getopts`, but `clap` is the most popular choice). What they need is a guide about 

* How to properly build your app using clap
* Best practices
* The features people use most (groups, subcommands, raw arguments, conflicts, requirements)

Instead, they get a few quick examples from the Readme, a few more elaborative examples, and a **big** set of methods they need to *glue together not yet knowing how things work here*. This makes the learning curve even more abrupt. 

### How to fix

We need something like [rust-cli book](https://rust-cli.github.io/book/index.html), but centered on CLI args' parsing part and clap-specific.

- [ ] Sweep through the readme and make it *shorter*. Readme is an advertisement, not documentation.
- [ ] The book.
- [ ] Cross-link mutually related methods on docs.rs. Make descriptions more elaborative.


---

_Label `C: docs` added by @CreepySkeleton on 2020-03-18 00:29_

---

_Label `T: RFC / question` added by @CreepySkeleton on 2020-03-18 00:31_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-03-18 00:31_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-03-18 00:31_

---

_Comment by @pksunkara on 2020-03-27 13:04_

As first part, can we emphasise gitter chat and have a small FAQ in the issue template?

---

_Comment by @CreepySkeleton on 2020-03-27 14:05_

I'd rather go with something like
```
MAKE SURE YOU HAVE ASKED YOU QUESTION ON <link to chat> FIRST. 
Maintainers are few, and their mental health is fragile.
```

Or something like that. Concise, draws attention, and, *I hope*, not rude.

---

_Comment by @pksunkara on 2020-03-27 14:06_

The first sentence is enough.

---

_Comment by @CreepySkeleton on 2020-03-27 14:07_

But the second explains why 😏 

---

_Comment by @pksunkara on 2020-03-27 14:07_

I don't think we need to.

---

_Comment by @CreepySkeleton on 2020-03-27 14:08_

 Anyway. I'm leaving the decision to you.

---

_Label `W: 3.x` removed by @pksunkara on 2021-05-26 14:37_

---

_Removed from milestone `3.0` by @epage on 2021-10-04 19:22_

---

_Assigned to @pksunkara by @pksunkara on 2021-10-27 10:17_

---

_Referenced in [epage/clapng#144](../../epage/clapng/issues/144.md) on 2021-12-06 19:14_

---

_Comment by @epage on 2021-12-08 21:01_

We've added a tutorial and cookbook.

---

_Closed by @epage on 2021-12-08 21:01_

---
