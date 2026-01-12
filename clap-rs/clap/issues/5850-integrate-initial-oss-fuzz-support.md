```yaml
number: 5850
title: Integrate initial OSS-Fuzz support
type: issue
state: closed
author: initializedd
labels:
  - C-enhancement
assignees: []
created_at: 2024-12-21T01:54:45Z
updated_at: 2025-04-29T15:36:24Z
url: https://github.com/clap-rs/clap/issues/5850
synced_at: 2026-01-12T16:14:17Z
```

# Integrate initial OSS-Fuzz support

---

_@initializedd_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

Initial support for fuzzing clap to discover and fix bugs.

If my fuzzer is merged, I will open a pull request in the [OSS-Fuzz repository](https://github.com/google/oss-fuzz) to run the fuzzers for this library on Google's infrastructure. Maintainers of clap will be notified if any bugs are discovered.

Please see the [OSS-Fuzz documentation](https://google.github.io/oss-fuzz/) and [Bug disclosure guidelines](https://google.github.io/oss-fuzz/getting-started/bug-disclosure-guidelines/) before merging.

Thanks!

### Describe the solution you'd like

PR #5851

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @initializedd on 2024-12-21 01:54_

---

_Referenced in [clap-rs/clap#5851](../../clap-rs/clap/pulls/5851.md) on 2024-12-21 01:55_

---

_Comment by @epage on 2024-12-24 02:42_

I've wondered about fuzzing but whats not too clear to me is what would provide the best value, or in other words, what are the high risk areas.  Like in #5851, its parsing strings (not `OsString`s) against a limited, static CLI definition.  I feel like this is unlikely to uncover anything of interest.

---

_Comment by @initializedd on 2024-12-24 13:47_

> I've wondered about fuzzing but whats not too clear to me is what would provide the best value, or in other words, what are the high risk areas. Like in #5851, its parsing strings (not `OsString`s) against a limited, static CLI definition. I feel like this is unlikely to uncover anything of interest.

Thanks for your response.

I agree that the fuzzer in #5851 is unlikely to uncover anything of interest, but I’d like to emphasize that that this is more about integrating OSS-Fuzz support than the specific fuzzers themselves at this stage. We need to ensure that we have the necessary approvals from both parties before diving deep into the fuzzers. The aim is to eventually reach 100% fuzzing coverage through an iterative process of constant refinement.

I've updated #5851 to parse `OsString`s instead.

I am open to any further suggestions you may have.

---

_Comment by @epage on 2025-01-02 20:45_

An `OsString` from a `String` doesn't make much of a difference.

For me, the big question is "will this be worth it" which is dependent on what a proposed fuzzer looks like.

---

_Comment by @popematt on 2025-01-07 16:54_

@initializedd, I'm sorry, I over-reacted based on my own bad experience with some unknown 3rd party actor.  I realize now that it sounds like I am accusing _you_ of malicious behavior. I know nothing about you, and so I cannot fairly comment on your intentions. In retrospect, I don't think my original comment has any value, so I will delete it.

---

_Comment by @initializedd on 2025-01-07 18:17_

> @initializedd, I'm sorry, I over-reacted based on my own bad experience with some unknown 3rd party actor. I realize now that it sounds like I am accusing _you_ of malicious behavior. I know nothing about you, and so I cannot fairly comment on your intentions. In retrospect, I don't think my original comment has any value, so I will delete it.

Thanks for understanding.

@epage Are there any specific examples that you would suggest drawing inspiration from to improve the proposed fuzzer?

---

_Comment by @epage on 2025-01-07 18:41_

I don't know of an example that would be relevant for fuzzing.  What I'm asking of you is to propose fuzzing that would provide enough value to be worth it.

---

_Comment by @initializedd on 2025-03-12 21:04_

@epage Apologies for my absence.

I am still keen to integrate fuzzing support for your library.

I've taken a further look at the codebase, and it seems to me that we will get the most value out of this by fuzzing the internal functions. Usages of `unsafe` (although few in the codebase) should probably be prioritized for extra assurance.

Some ideas of internal functions that we could start with:

- [is_number](https://github.com/clap-rs/clap/blob/master/clap_lex/src/lib.rs#L492)
- [is_negative_number](https://github.com/clap-rs/clap/blob/master/clap_lex/src/lib.rs#L432)
- [strip_prefix](https://github.com/clap-rs/clap/blob/master/clap_lex/src/ext.rs#L201)
- [split_once](https://github.com/clap-rs/clap/blob/master/clap_lex/src/ext.rs#L223)
- [split_at](https://github.com/clap-rs/clap/blob/master/clap_lex/src/ext.rs#L275)
- [next_value_os](https://github.com/clap-rs/clap/blob/352e99f59e7aabf2a34e5e9fd334e568a0b197e6/clap_lex/src/lib.rs#L453)

Absolutely happy to implement support for all of those (providing that you agree), and any more that you think would be good. 

---

_Comment by @epage on 2025-03-17 18:25_

Note that the goal for any of those `ext.rs` functions is for them to be moved into `std` and for us to use that, see rust-lang/libs-team#311

`clap_lex`s public API is a possibility, depending on what the fuzzing would like.

---

_Comment by @initializedd on 2025-04-28 22:18_

@epage I've updated the PR to fuzz `clap_lex` as suggested, specifically `ShortFlags::next_value_os`, what are your thoughts?

---

_Comment by @epage on 2025-04-29 15:36_

In re-evaluating this, I feel like fuzzing is not offering enough of a benefit.

---

_Closed by @epage on 2025-04-29 15:36_

---
