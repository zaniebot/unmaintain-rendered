```yaml
number: 21283
title: Would this project accept a PR that makes the number of top-level blank lines configurable?
type: issue
state: closed
author: arjun-menon
labels: []
assignees: []
created_at: 2025-11-05T14:52:14Z
updated_at: 2025-11-05T14:59:22Z
url: https://github.com/astral-sh/ruff/issues/21283
synced_at: 2026-01-10T11:10:00Z
```

# Would this project accept a PR that makes the number of top-level blank lines configurable?

---

_Issue opened by @arjun-menon on 2025-11-05 14:52_

### Summary

# What I'm Proposing

I would like to make a tiny PR that makes this const configurable (**but defaulting to `2` of course**):

https://github.com/astral-sh/ruff/blob/7569b09bdd8dfa4587e44cfbed63fa71ca361261/crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs#L26-L27

Would this project accept such a PR?

# About PEP 8 on this proposal

I'm aware that [PEP 8 says](https://peps.python.org/pep-0008/#blank-lines):

> Surround top-level function and class definitions with two blank lines.

However, this is a problematic PEP 8 recommendation as it takes up valuable vertical screen real estate.

It especially creates a major ergonomic issue with modern monitors, which tend to be wide (often with an aspect ratio of 16:9).

Also, no other major programming language (e.g. C, C++, Java, Kotlin, Rust, etc.) have a recommendation like this for top-level blank lines.

# About PEP and other existing Ruff rules

A major reason I switched to `ruff` is the very flexible configurability.

There are other PEP 8 recommendations that I personally do not agree with.

For example, PEP 8 says under _[Tabs or Spaces?](https://peps.python.org/pep-0008/#tabs-or-spaces)_ that _"Spaces are the preferred indentation method."_. However, [my `.ruff.toml`](https://github.com/arjun-menon/alteza/blob/master/.ruff.toml) for all my project explicitly say `indent-style = "tab"`. I use tabs simply because it's easier to jump over with my keyboard. I can go up/down indentation with 1x key button press per level versus 4 presses.

Also, for example, under _[Maximum Line Length](https://peps.python.org/pep-0008/#maximum-line-length)_, PEP 8 also says: _"Limit all lines to a maximum of 79 characters."_, but I've configured ruff with `line-length = 120` against that recommendation, simply as monitors are wider today.

All that is to say is that (I hope) that we should take PEP 8 recommendations with a grain of salt. Code style is an inherently subjective matter, and not everyone necessarily agrees with PEP 8 recommendations.

## Endnote

Even if only used by a handful of people, I think this proposed rule (maybe called `top-level-blank-lines`) would truly be beneficial and a positive impact for the people that use it.

We already have rules that allow people to deviate from PEP 8 guidelines (like `indent-style` and `line-length`), so I'm hoping that making this configurable would be okay. Please let me know if this project would accept a PR on this. Thank you.

---

_Comment by @MichaReiser on 2025-11-05 14:56_

Thanks for the detailed write up and checking in before opening a PR. 

Unfortunately, such an option would be incompatible with the formatter. Formatter/linter compatibility is important to us because our long-term goal is to automatically run the formatter after the linter (with the option to opt out).  Any lint rule or configuration that is incompatible with the formatter (in the sense that the formatter could introduce new violations) makes this more challenging, which is why we aren't accepting any such rules or options at the moment. 

---

_Closed by @MichaReiser on 2025-11-05 14:56_

---
