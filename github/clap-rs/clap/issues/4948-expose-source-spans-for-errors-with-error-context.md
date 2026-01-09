---
number: 4948
title: Expose source spans for errors with error-context
type: issue
state: open
author: TzviPM
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2023-06-04T13:48:00Z
updated_at: 2025-01-28T22:19:14Z
url: https://github.com/clap-rs/clap/issues/4948
synced_at: 2026-01-07T13:12:20-06:00
---

# Expose source spans for errors with error-context

---

_Issue opened by @TzviPM on 2023-06-04 13:48_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

I'm building a compiler front-end and I want to use a common diagnostic format for all of the errors, whether in the input source code with linting or parsing or analysis, and I want this to extend also to the cli arguments. For some kinds of clap errors, such as InvalidValue, it would be nice to show the context around the argument based on the command that the user typed. As far as I can tell, clap does not currently support this use case.

I could write my own parser for the command line, but it would be nice to reuse the work that clap has done, even with my own error display logic.

### Describe the solution you'd like

Clap already has a crate feature called `error-context` that supports most of what I need. I propose adding a new context kind that supplies some sort of source span for errors where that is relevant.

### Alternatives, if applicable

_No response_

### Additional Context

I have used clap for many years and love the work you've done. I would be happy to contribute by writing a PR for this feature of the maintainers would be willing to accept it.

---

_Label `C-enhancement` added by @TzviPM on 2023-06-04 13:48_

---

_Comment by @epage on 2023-06-05 14:42_

I know at least #2836 made this more difficult before.  With that gone, it is a bit easier.

If we can generally point to an argument, that makes things a bit easier.  If we need to point to a substring in the argument (which short flag, the part after a `=`, etc), that increases the complexity.

A downside this that this pushes extra bookkeeping on everyone, performance and code-size wise with the more complex it is the more it affects things.  We have talked about making the parser pluggable (#2468) which could open the door for this being more opt-in

---

_Label `A-parsing` added by @epage on 2023-06-05 14:42_

---

_Label `S-waiting-on-design` added by @epage on 2023-06-05 14:42_

---

_Referenced in [clap-rs/clap#5883](../../clap-rs/clap/pulls/5883.md) on 2025-01-17 00:52_

---

_Comment by @jordanjennings-mysten on 2025-01-17 19:26_

As a simple useful first step can we pass back the index of the arg relevant to an error? Reduces the complexity and allows users to calculate spans on their own, low overhead with a single integer. For any corner cases, such as lists which have unparsable values we could make sure there is enough context to point to the exact location, but for the first pass it could be a big win just to know which arg caused the problem.

To keep the interface changes to a minimum `impl Into -> something` could be used on the function signature so we only chain public interfaces once but allow more information to be added as needed. Or maybe just need an intermediate type here which today only has the index.


---

_Comment by @epage on 2025-01-28 22:19_

It would help to know from multiple perspectives if just the arg index is sufficient so we don't design ourselves into a corner.

This still doesn't help with the bookkeeping side of this.  Its easier to track the index for errors during parsing.  For errors during validation, we'd have to add this index to `ArgMatches` and report that back up.  We'd also need to understand if other args are involved in an error, if we need to track their index.  If the error is related to a group, what index, do we use, etc.

I want to be clear that I overall put this as a low priority and will be slow to respond as I'm not seeing enough value from this and a lot of  potential costs and need to focus my time in other areas.

---
