---
number: 5044
title: "`ContextKind::Custom` has no functionality"
type: issue
state: open
author: benluelo
labels: []
assignees: []
created_at: 2023-07-24T23:45:44Z
updated_at: 2023-07-25T00:38:27Z
url: https://github.com/clap-rs/clap/issues/5044
synced_at: 2026-01-07T13:12:20-06:00
---

# `ContextKind::Custom` has no functionality

---

_Issue opened by @benluelo on 2023-07-24 23:45_

As per the title, [`ContextKind::Custom`](https://github.com/clap-rs/clap/blob/1f022b83958334241c47de10195a2a502d99d81b/clap_builder/src/error/context.rs#L62) is currently useless as nothing ever reads it. I expected it to be used in a similar way to [`ContextKind::Usage`](https://github.com/clap-rs/clap/blob/master/clap_builder/src/error/format.rs#L118-L121), in that it could be attached to any ErrorKind and be printed as [An opaque message to the user](https://github.com/clap-rs/clap/blob/1f022b83958334241c47de10195a2a502d99d81b/clap_builder/src/error/context.rs#L38), but it just does nothing.



---

_Comment by @epage on 2023-07-24 23:59_

Huh, that must have slipped through the cracks in #3402 (real don't want to dig into the 35 commits to verify)

Our options
- Reserve it for use for custom formatters
- Define how it should be rendered in our errors
- Remove it on the next breaking change

---

_Comment by @benluelo on 2023-07-25 00:05_

I think defining how it should be rendered would be the best option - it would be super helpful for adding additional context as to why a custom ValueParser failed ("cannot parse '0xinvalid' as hex", "file 'abc.xyz' should have .txt extension", etc)

---

_Comment by @epage on 2023-07-25 00:09_

If its for a random `ValueParser`, why not just include it in the main message?

How would it show up in the regular error formatting:
```
error: Some messge

Usage: ...

For more information, try `--help`
```

---

_Comment by @benluelo on 2023-07-25 00:26_

A few things:

- I couldn't get `ContextKind::Usage` to work, but I just realized that it's because it expects a `ContextValue::StyledStr`. (documentation on what values are valid in what cases would be great!)

- I would expect `ContextKind::Usage` to prepend `Usage: ` to the provided `ContextValue::StyledStr`, considering it's doc comment of "A usage string", [which it does not(?)](https://github.com/clap-rs/clap/blob/master/clap_builder/src/error/format.rs#L468)

- Even with the above, I would still expect a way to append a general "context" to the error message, along with the usage. Is it expected that this is all done in the usage message, manually formatted?

---

_Comment by @epage on 2023-07-25 00:38_

>   I couldn't get ContextKind::Usage to work, but I just realized that it's because it expects a ContextValue::StyledStr. (documentation on what values are valid in what cases would be great!)


While it would be great to have everything in clap to the same degree of quality, the practical aspect is that this area isn't as much of a focus as this is fairly off the beaten path.  So long as we provide an escape hatch to work, that is sufficient. 

If there is interest from someone improving the docs in a way that fits within the docs and doesn't detract from higher priority workflows, it would be welcome.

> I would expect ContextKind::Usage to prepend Usage: to the provided ContextValue::StyledStr, considering it's doc comment of "A usage string", [which it does not(?)](https://github.com/clap-rs/clap/blob/master/clap_builder/src/error/format.rs#L468)

We can consider this for 5.0 though it should have its own Issue

> Even with the above, I would still expect a way to append a general "context" to the error message, along with the usage. Is it expected that this is all done in the usage message, manually formatted?

What do you mean?

---
