```yaml
number: 5199
title: Document how to use ArgGroups from derive API
type: issue
state: open
author: kpreid
labels:
  - C-enhancement
assignees: []
created_at: 2023-11-07T18:50:46Z
updated_at: 2023-11-27T22:21:07Z
url: https://github.com/clap-rs/clap/issues/5199
synced_at: 2026-01-12T16:14:16Z
```

# Document how to use ArgGroups from derive API

---

_@kpreid_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.4.7

### Describe your use case

Context: 

* I want to make a set of mutually exclusive options
* I understand that `ArgGroup`s serve this purpose
* I am using `derive(clap::Parser)`

Problem:

The documentation does not contain any examples how to achieve this or describe the full picture. The only discussion of is the "[ArgGroup Attributes](https://docs.rs/clap/4.4.7/clap/_derive/index.html#arggroup-attributes)" which mentions in passing that "These correspond to the `ArgGroup` which is implicitly created for each Args derive.", from which I can infer that the solution is likely to create a nested struct for the group (and I later determined this succeeded).

I tried `#[arg(group = "action", conflicts_with = "action")]` on each arg to see if that worked, but (not too surprisingly) it made the args conflict with themselves.

### Describe the solution you'd like

* Provide at least one discoverable example of using `ArgGroup` features such as mutual exclusion in a derive-only program.
* Clarify in reference documentation that in order to make a configured group via derive, you _must_ make a `derive(Parser)` struct for that group.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @kpreid on 2023-11-07 18:50_

---

_Comment by @kpreid on 2023-11-07 19:11_

Also, the `ArgGroup` documentation says that [`multiple = false` is the default](https://docs.rs/clap/4.4.7/clap/struct.ArgGroup.html), but the `derive(Parser)` implicit group defaults to `multiple = true` and the derive reference does not mention this.

---

_Referenced in [clap-rs/clap#5200](../../clap-rs/clap/pulls/5200.md) on 2023-11-07 19:47_

---

_Comment by @epage on 2023-11-07 19:49_

A part of the tutorial does cover [mutually exclusive arg group](https://docs.rs/clap/latest/clap/_derive/_tutorial/chapter_3/index.html#argument-relations).

#5200 documents the implicit behavior.

Ideally we'd have #2621 which would make this more natural.

---

_Comment by @kpreid on 2023-11-08 04:59_

> A part of the tutorial does cover [mutually exclusive arg group](https://docs.rs/clap/latest/clap/_derive/_tutorial/chapter_3/index.html#argument-relations).

Ah. To give more detail: only when writing up this issue did I discover that the tutorial even *had* multiple chapters, and while I did skim through them I didn't search every chapter thoroughly (and to my thought process, "validation" meant "of values", not "of all states args can be in"). And, when I'm trying to solve a problem, I generally don't go for tutorials first, but for API-documentation (in the sense of the kind of thing rustdoc is mainly used for).

I still think that it would be an improvement if searching the derive reference page for “ArgGroup” turned up a sentence that communicated that the _only_ way to create groups, that have nondefault options and don't contain every arg, is with a separate struct.


---

_Comment by @epage on 2023-11-08 19:08_

> Ah. To give more detail: only when writing up this issue did I discover that the tutorial even had multiple chapters, and while I did skim through them I didn't search every chapter thoroughly (and to my thought process, "validation" meant "of values", not "of all states args can be in"). 

It was originally all in one page and people were overwhelmed by it and complaining.  #5163 is about starting it on the table of contents rather than the first page.

> I still think that it would be an improvement if searching the derive reference page for “ArgGroup” turned up a sentence that communicated that the only way to create groups, that have nondefault options and don't contain every arg, is with a separate struct.

You can always create a group manually (`#[command(group = ArgGroup::new(...)...)]`).  That is covered by [explaining raw attributes and linking to the API](https://docs.rs/clap/4.4.7/clap/_derive/index.html#command-attributes).

In the [overview](https://docs.rs/clap/4.4.7/clap/_derive/index.html#overview), we do show a separate struct with `#[group]` attributes.

When discussing [ArgGroup](https://docs.rs/clap/4.4.7/clap/_derive/index.html#arggroup-attributes), we do have this

> These correspond to the [ArgGroup](https://docs.rs/clap/4.4.7/clap/struct.ArgGroup.html) which is implicitly created for each Args derive.

Maybe examples would help?  I don't want to commit to examples for everything (see #4090).  Maybe linking to a part in the tutorial that is most relevant for each type of attribute?

---

_Comment by @kpreid on 2023-11-08 19:26_

I think an example would help (and I'd justify writing one with "groups are not just a single option but an entire kind of entity"), and cross-references to the tutorial would help.

---

_Referenced in [clap-rs/clap#5229](../../clap-rs/clap/pulls/5229.md) on 2023-11-27 22:17_

---

_Comment by @epage on 2023-11-27 22:21_

#5229 adds links for each attribute type to the relevant tutorial sections.

---
