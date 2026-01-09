---
number: 5962
title: "Docs: Add a derive example for Help where it's disabled"
type: issue
state: open
author: joshka
labels:
  - C-enhancement
  - A-docs
assignees: []
created_at: 2025-03-28T14:34:17Z
updated_at: 2025-03-31T14:11:42Z
url: https://github.com/clap-rs/clap/issues/5962
synced_at: 2026-01-07T13:12:20-06:00
---

# Docs: Add a derive example for Help where it's disabled

---

_Issue opened by @joshka on 2025-03-28 14:34_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5

### Describe your use case

In an old CLI I'm upgrading from clap 2, the current cli uses -h for a host param, so this can't be used for help.
It took me a bit of digging to get this to work right. My roadblocks were missing docs in the derive tutorial / reference for disable_help_flag and the help related ArgActions.

```rust
#[derive(Debug, Parser)
#[command(disable_help_flag = true)
struct Args {
    // The hostname
    #[arg(short, long)
    host: Uri,

    // Print help
    #[arg(long, action = ArgAction::HelpLong)]
    help: Option<bool>,
}
```


### Describe the solution you'd like

Add the following docs to the derive tutorial and reference:
1. disable_help_flag
2. ArgAction::{Help, HelpShort, HelpLong}
3. The need to use `Option` when using one of those variants


### Alternatives, if applicable

Something that could be worth considering as a wider effort that might make it easier to find each of these docs is adding the derive code as examples to each of the various enums / options (e.g. ArgAction). This would be in addition to the current examples which only show the builder code, and it would make it clear how the builder and the derive syntax are aligned generally.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @joshka on 2025-03-28 14:34_

---

_Label `A-docs` added by @epage on 2025-03-28 17:37_

---

_Comment by @epage on 2025-03-28 17:44_

We frequently get requests for a problem someone ran into to be specifically called out in the documentation and I generally push for there to be a specific reason why that problem should be called out compared to any other, and to instead rely in principles otherwise, to help keep the documentation a manageable size so people can discover the content in there (rather than being even more overwhelmed than they already are) and to help with maintenance.

I think the particular problem here is the non-obvious way of adapting the principles to the derive API, that `Option<bool>` is needed or `()` with a value parser.

btw our test for this, using `()`, is at 
https://github.com/clap-rs/clap/blob/master/tests/derive/help.rs#L438-L452

`Option<bool>` does shorten it a bit.

> Something that could be worth considering as a wider effort that might make it easier to find each of these docs is adding the derive code as examples to each of the various enums / options (e.g. ArgAction). This would be in addition to the current examples which only show the builder code, and it would make it clear how the builder and the derive syntax are aligned generally.

This would fall more squarely under #4090.

---

_Comment by @epage on 2025-03-28 17:45_

> I think the particular problem here is the non-obvious way of adapting the principles to the derive API, that Option<bool> is needed or () with a value parser.

If we move are considering forward with this, the question then becomes where would this best fit within the documentation such that it is discoverable and fits the flow (with any of the caveats of #4090 applied)

---

_Comment by @joshka on 2025-03-28 23:32_

Taking the step back from the general solution much and solving just this problem, it's clear to me that a derive example of each ArgAction belongs in https://docs.rs/clap/latest/clap/enum.ArgAction.html and a derive example for the use of the action attribute belongs in https://docs.rs/clap/latest/clap/struct.Arg.html#method.action. This makes these docs available in the search results in docs.rs without having to text search withing some large non-idiomatic narrative. What are the downsides of just that approach (aside from the work to make it happen)?

> btw our test for this, using (), is at
https://github.com/clap-rs/clap/blob/master/tests/derive/help.rs#L438-L452

This didn't come up in github searches as I was looking specifically for ShortHelp which has no example code. https://github.com/search?q=repo%3Aclap-rs%2Fclap+helpshort&type=code

My search path for this was docs.rs derive tutorial, reference, trying to understand how the action stuff worked from the docs, trying to look for code that showed this in examples, using just a bool for the field and not knowing why it wasn't interpreting as false when the flag wasn't set, trying setting the default value to false, giving up and commenting it out before eventually trying a Option<bool>. All told, this was many many steps rather than search, find example, implement.

---

_Comment by @epage on 2025-03-31 14:11_

> Taking the step back from the general solution much and solving just this problem, it's clear to me that a derive example of each ArgAction belongs in https://docs.rs/clap/latest/clap/enum.ArgAction.html and a derive example for the use of the action attribute belongs in https://docs.rs/clap/latest/clap/struct.Arg.html#method.action. This makes these docs available in the search results in docs.rs without having to text search withing some large non-idiomatic narrative. What are the downsides of just that approach (aside from the work to make it happen)?

It is not clear to me why this specific attribute / enum should have it while none of the other ones should.  I'd like to focus on the general problem.

---
