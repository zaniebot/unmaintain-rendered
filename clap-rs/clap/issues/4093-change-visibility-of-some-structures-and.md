```yaml
number: 4093
title: Change visibility of some structures and functions for config-rs integration
type: issue
state: closed
author: jgerrish
labels:
  - C-enhancement
assignees: []
created_at: 2022-08-19T04:51:39Z
updated_at: 2022-08-24T21:13:51Z
url: https://github.com/clap-rs/clap/issues/4093
synced_at: 2026-01-12T16:14:15Z
```

# Change visibility of some structures and functions for config-rs integration

---

_@jgerrish_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

I'm writing a source for the config-rs crate to integrate clap command line parsing options.

The config crate provides a wrapper to manage configuration variables from TOML, environment variables and other sources.  I added clap as a source so that command line arguments can be retrieved like other configuration variables.

To achieve this, I needed to make some crate visible structures and functions visible to the public.

Thank you.

### Describe the solution you'd like

I'd like to make some more structures and functions visible to the public, specifically MatchedArg, the type_id functions, and a new function to list all the command line arguments.

Here's the code that shows the use case:
https://github.com/jgerrish/config-rs/blob/clap-config-source/src/clap/source.rs

Here is my branch of clap with the proposed changes:
https://github.com/clap-rs/clap/compare/master...jgerrish:clap:public-get-keys

### Alternatives, if applicable

Instead of making those structures visible, you could provide additional public functions to access the functionality.  I'm open to an alternative solution if you think the visibility change is too lenient.



### Additional Context

_No response_

---

_Label `C-enhancement` added by @jgerrish on 2022-08-19 04:51_

---

_Comment by @epage on 2022-08-19 13:25_

In general, see also https://github.com/clap-rs/clap/discussions/2763

> MatchedArg

Can you say what making this public would offer over what the current API provides?

> type_id 

Again, what is it that this helps with?

> new function to list all the command line arguments

This was tracked in https://github.com/clap-rs/clap/issues/1206.  It was blocked on a clap v4 change but now is resolved.

---

_Comment by @jgerrish on 2022-08-20 04:24_

@epage 

> type_id

> Again, what is it that this helps with?

Thank you for asking, I'll give the high-level reason and the tech reason here:

clap is a great library for command line parsing, and the config crate let's developers integrate environment and config data into their application.  To support both config files and command line argument overrides (or vice versa), I'm integrating them both into a common pipeline that can offer flexible rules for priorities, defaults, etc.  For example, I want configuration settings to normally come from config files, but if I run a service manually with command line parameters, I want it overridden with worrying about that program logic in every app I write.

config-rs, like clap, has type information for configuration settings.  For example, debug may be a boolean variable and asset path or some thing might be a string.

To use clap as a source, we need to map clap's types to config-rs types, with some optional custom mangling if needed.

For example, if I have a --tag command line parameter that is ArgAction::Append, I may need to call get_many on it.  But for something like --verbose, get_one is appropriate.  I also need to include the type parameter when I make those calls.

This information isn't known to the config-rs application without a type_id or exposing some other API.  I may be missing some API features, if so please let me know and I can use them.

Thank you again for your help.


---

_Comment by @epage on 2022-08-20 16:13_

@jgerrish is accessing `AnyValueId` sufficient or does it have to be `TypeId`?  If so, why?

While not the cleanest, would getting it via a `MatchesError::Downcast` be sufficient?  For example, you could just do a `get_many::<()>`, get the error, and then do the `get_many` with the right type.

I don't say this as an ideal API but
- This allows access immediately, not being blocked on any API discussions
- We tend to focus cleaner APIs for parts commonly used by end users and everyone else gets primitives to build the solution they need

---

_Comment by @jgerrish on 2022-08-21 01:25_

@epage 

> @jgerrish is accessing AnyValueId sufficient or does it have to be TypeId ? If so, why?

AnyValueId has private structure members, and I just need the type info.  I was following what I thought were practices in your code base of providing a getter there, but down below I see you mention different approaches for users and developers.

> While not the cleanest, would getting it via a MatchesError::Downcast be sufficient? For example, you could just do a get_many::<()> , get the error, and then do the get_many with the right type.

I don't say this as an ideal API but

• This allows access immediately, not being blocked on any API discussions

You're right, it's not clean.  But thank you for the pointer to Error::Downcast.  Errors are one of those Rust features that I should get more comfortable with.  The ? and matches operator and all it entails make development wonderful.

It sounds like you need to go through more API discussions.  I'll keep my code pointed to a fork and check back periodically.  That shouldn't change any cost benefit analysis anywhere, since I'm the only current user.

Feel free to close this issue, and thank you for taking the time for feedback.  It's appreciated and have a good weekend.



---

_Comment by @epage on 2022-08-21 01:42_

> AnyValueId has private structure members, and I just need the type info. 

`AnyValueId` is a form of type info.  What I'm trying to understand is why it won't work for within your design. 

Oh, `of` isn't public.  Would that being public be enough?

---

_Comment by @jgerrish on 2022-08-24 21:12_

Not enough, but thank you.  I assume if there is need others will ask.  If not, your closed practices are best.

---

_Closed by @jgerrish on 2022-08-24 21:13_

---

_Comment by @jgerrish on 2022-08-24 21:13_

Thank you for the review and time.

---
