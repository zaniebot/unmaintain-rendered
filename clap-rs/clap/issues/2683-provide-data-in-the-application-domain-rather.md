```yaml
number: 2683
title: Provide data in the application domain rather than the CLI domain
type: issue
state: closed
author: pksunkara
labels:
  - C-enhancement
  - M-breaking-change
  - A-builder
  - A-parsing
  - A-derive
  - S-waiting-on-mentor
assignees: []
created_at: 2021-08-13T01:05:10Z
updated_at: 2022-05-23T15:33:39Z
url: https://github.com/clap-rs/clap/issues/2683
synced_at: 2026-01-12T16:14:13Z
```

# Provide data in the application domain rather than the CLI domain

---

_@pksunkara_

Currently, clap's API is focused on the CLI domain, with data being tracked as `OsString` with specialized helpers for `String`.  At the tale-end of the builder API, we provide the `value_of_t` API.  We do gloss over this in the derive API.

Problems with the current approach
- Bloat of functions in `ArgMatches` to handle every case
- It is easy to use `_os` functions but disallow non-UTF8 during validation (https://github.com/clap-rs/clap/pull/2677)
- Derive API parses data twice, once as part of validation and then when giving it to the user (#2206, #3589)
- Most builder users won't bother with the extra development effort of validating like the derive API and instead will not get first class clap errors for their parse errors

See also https://github.com/clap-rs/clap/discussions/2832, #1185 

Roll out
1. Change the data structure for `ArgMatches` to store `Box<Any + Send + Sync + 'static>`
2. Add a generic getter, gated behind a feature flag
3. Add a builder function for setting a custom value, gated behind a feature flag
4. Change the default parser from `OsString` to `String`, deprecating `AllowInvalidUtf8`, gated behind a feature flag
5. Mark old API deprecated, gated behind a feature flag
6. On next breaking release, remove deprecated APIs and remove feature gates

Other considerations / open questions
- For `multiple_values`, users also have to set `default_values`, `value_names`, and `value_hints`, should we follow `possible_values` and avoid users passing in parallel arrays?
- How do we handle default values?
  - Users will want to set their defaults according to their application data type
  - To show a default in help, we need it to be `Display`
  - `clap_derive` currently punts on this by providing both a `default_value` and `default_value_t` where the latter has to support `Display` and it gets forwarded to `default_value`, see https://github.com/clap-rs/clap/pull/2635
- How many feature gates and when do we remove them?
  - The sooner its public, the better, for collecting feedback
  - The more people using it, the more impacted by subtle behavior changes (default parser)
    - We could put in hacks to make the old API work along side the new API and set the default parser according to `AllowInvalidUtf8`
- How do we handle optional vs required, especially in light of #2505?
  - We have to deal with present / not-present, invalid cast of `Any`, etc.
- For the parser, how do we balance flexibility and performance
  - We could make the parser a `trait ValueParser` and provide a `struct BoxedValueParser` that contains an enum for common function-pointer signatures (wrapped with converters for the error types) and people can explicitly construct a `BoxedValueParser` for their custom type
- In designing the API, we should keep in mind that people are also wanting access to what authored a value (env, default, CLI) (**TODO** Find Issue)
  - Unsure how this feature would even fit into `clap_derive`
- Long term, we will want to morph the `ArgMatches` API to be more map-like
  - #1206 
  - Having a `remove` function would allow `clap_derive` to move values, instead of clone them.
- Long term, we need to consider other type-related features besides `default_value`:
  - `ValueHint`: if we can detect and set a default for `Path` that would provide a better default experience
  - `value_name` for named arguments: the default (of `--name <name>`) is redundant.  If we could provide type related defaults (e.g. `PATH` for `PathBuf`, `NUM` for numbers), it would  also provide a better default experience
  - See https://github.com/clap-rs/clap/discussions/2637 for prior discussion

---
*Original*

Basically, there are two things that need to be solved here.

* We should be keeping the parsed values during validation stage of parsing instead of throwing them away
* We should be storing the types directly in ArgMatches

I have to dig into the code to write a longer description of issues with the current behaviour and design.

- [ ] #2505
- [ ] #2206
- [ ] #2298
- [x] #3589
- [x] #2437


See also https://github.com/clap-rs/clap/discussions/2832

---

_Label `C: matches` added by @pksunkara on 2021-08-13 01:05_

---

_Label `C: validators` added by @pksunkara on 2021-08-13 01:05_

---

_Added to milestone `3.0` by @pksunkara on 2021-08-13 01:05_

---

_Renamed from "Validator & ArgMatches issues epic" to "Validator & ArgMatches need to be reworked?" by @pksunkara on 2021-08-13 01:08_

---

_Referenced in [clap-rs/clap#2505](../../clap-rs/clap/issues/2505.md) on 2021-10-13 17:50_

---

_Removed from milestone `3.0` by @epage on 2021-10-16 20:03_

---

_Added to milestone `4.0` by @epage on 2021-10-16 20:03_

---

_Comment by @epage on 2021-10-16 20:11_

Sounds like this is related to https://github.com/clap-rs/clap/discussions/2832

---

_Referenced in [clap-rs/clap#2437](../../clap-rs/clap/issues/2437.md) on 2021-10-16 20:38_

---

_Referenced in [clap-rs/clap#2908](../../clap-rs/clap/pulls/2908.md) on 2021-10-18 21:23_

---

_Renamed from "Validator & ArgMatches need to be reworked?" to "Provide data in the application domain rather than the CLI comain" by @epage on 2021-10-18 21:25_

---

_Renamed from "Provide data in the application domain rather than the CLI comain" to "Provide data in the application domain rather than the CLI domain" by @epage on 2021-10-18 21:31_

---

_Label `E: breaking change` added by @epage on 2021-10-18 22:08_

---

_Comment by @epage on 2021-10-18 22:25_

@pksunkara just captured in the Issue my thoughts from not being able to sleep last night :)  I'm sure you also have some and would love to hear and see how we can synthesize the two.

The only linked issue I'm seeing this directly help with is #2206.  I thought it would help with #2505 but it sounds like we just repeat the same problem after exploring the idea.

---

_Comment by @pksunkara on 2021-10-18 22:55_

Looks good to me. I would actually make this the main product objective for 4.0. I will add more if needed once I get into this context later (which I still have in my todo)

---

_Referenced in [clap-rs/clap#2813](../../clap-rs/clap/issues/2813.md) on 2021-11-04 16:10_

---

_Referenced in [clap-rs/clap#1506](../../clap-rs/clap/issues/1506.md) on 2021-11-04 16:12_

---

_Referenced in [clap-rs/clap#1365](../../clap-rs/clap/issues/1365.md) on 2021-11-04 16:47_

---

_Referenced in [clap-rs/clap#2924](../../clap-rs/clap/issues/2924.md) on 2021-11-04 18:40_

---

_Referenced in [clap-rs/clap#2991](../../clap-rs/clap/issues/2991.md) on 2021-11-04 21:39_

---

_Comment by @epage on 2021-11-05 16:48_

From https://github.com/clap-rs/clap/discussions/2832

@ssokolow 
> Hmm. My main concern there is that this line looks like it could easily become a footgun if not handled carefully:
> 
> > Change the default parser from `OsString` to `String`, deprecating `AllowInvalidUtf8`, gated behind a feature flag.
> 
> Windows may make it difficult to get paths with un-paired surrogates, but I've found [mojibake](https://en.wikipedia.org/wiki/Mojibake)'d filenames to be easy on POSIX platforms... to the point where it led me to report a bug in Serde because a mojibake'd Japanese ID3 tag had put mojibake'd characters in media file names during auto-renaming and Serde doesn't complain if you want to put a `PathBuf` into a JSON file without explicitly specifying how to handle un-UTF8-able contents. (Was it ID3v1 that left the tag's encoding to be communicated out-of-band? I think Audacious Media Player actually has a browser-style encoding detector for that.)



---

_Comment by @epage on 2021-11-05 16:50_

> Hmm. My main concern there is that this line looks like it could easily become a footgun if not handled carefully:

With the proposed API, if you want a path, you'd be specifying it as a `PathBuf` and we would parse from `OsStr`.  Just if you didn't specify any custom type, we'd be choosing `String` by default which seems to be what people want in most cases.

---

_Comment by @ssokolow on 2021-11-05 17:05_

Ahh. By "if not handled carefully", I meant "this could turn into a footgun for end users if clap doesn't handle it carefully", and that should fit the definition of "clap handling it carefully" well enough.

---

_Referenced in [clap-rs/clap#3029](../../clap-rs/clap/pulls/3029.md) on 2021-11-15 19:10_

---

_Referenced in [clap-rs/clap#2943](../../clap-rs/clap/pulls/2943.md) on 2021-11-22 18:52_

---

_Referenced in [epage/clapng#36](../../epage/clapng/issues/36.md) on 2021-11-28 01:40_

---

_Referenced in [epage/clapng#196](../../epage/clapng/issues/196.md) on 2021-12-06 21:19_

---

_Referenced in [epage/clapng#211](../../epage/clapng/issues/211.md) on 2021-12-06 22:15_

---

_Referenced in [epage/clapng#229](../../epage/clapng/issues/229.md) on 2021-12-06 22:22_

---

_Referenced in [epage/clapng#233](../../epage/clapng/issues/233.md) on 2021-12-06 22:23_

---

_Label `C: matches` removed by @epage on 2021-12-08 20:21_

---

_Label `C: validators` removed by @epage on 2021-12-08 20:21_

---

_Label `A-builder` added by @epage on 2021-12-08 20:21_

---

_Label `A-derive` added by @epage on 2021-12-08 20:21_

---

_Label `A-parsing` added by @epage on 2021-12-08 20:21_

---

_Referenced in [clap-rs/clap#1704](../../clap-rs/clap/issues/1704.md) on 2021-12-08 21:02_

---

_Referenced in [clap-rs/clap#1723](../../clap-rs/clap/issues/1723.md) on 2021-12-08 21:15_

---

_Referenced in [clap-rs/clap#1185](../../clap-rs/clap/issues/1185.md) on 2021-12-09 18:10_

---

_Referenced in [clap-rs/clap#1682](../../clap-rs/clap/issues/1682.md) on 2021-12-09 20:40_

---

_Referenced in [clap-rs/clap#2122](../../clap-rs/clap/issues/2122.md) on 2021-12-13 15:02_

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-13 17:09_

---

_Label `C-enhancement` added by @epage on 2021-12-13 22:44_

---

_Referenced in [clap-rs/clap#3008](../../clap-rs/clap/issues/3008.md) on 2022-01-10 16:24_

---

_Referenced in [clap-rs/clap#3266](../../clap-rs/clap/issues/3266.md) on 2022-01-11 16:56_

---

_Referenced in [clap-rs/clap#3344](../../clap-rs/clap/issues/3344.md) on 2022-01-25 18:44_

---

_Referenced in [clap-rs/clap#3199](../../clap-rs/clap/issues/3199.md) on 2022-02-02 20:07_

---

_Referenced in [clap-rs/clap#3459](../../clap-rs/clap/issues/3459.md) on 2022-02-13 02:43_

---

_Referenced in [clap-rs/clap#3496](../../clap-rs/clap/issues/3496.md) on 2022-02-22 15:22_

---

_Referenced in [clap-rs/clap#3589](../../clap-rs/clap/issues/3589.md) on 2022-03-29 14:30_

---

_Referenced in [clap-rs/clap#3405](../../clap-rs/clap/issues/3405.md) on 2022-05-09 21:54_

---

_Comment by @AndreasBackx on 2022-05-11 12:59_

@epage requested I share our usecases after a short chat.

We are logging various aspects of CLI usage across the company. This is to give CLI owners an understanding of how their CLIs are used. This discoverability gives various teams the insights to improve their CLIs and see where they could improve. One example of this is the ability to see which subcommands are most often used and how long they take. This can help a team prioritise making improvements to a CLI. Saying "We saved 1000 engineers a total of 50 hours a week." is more impactful and provides incentives towards CLI improvements.

Additionally this information can be useful when debugging errors as well. We are currently working towards providing automated error logging. This means that when their CLI crashes, the user doesn't have to do anything and it'll be appropriately logged and can be followed up on. Having a better understanding of the state of the CLI can be beneficial. Maybe your parsing of a CLI argument lead to the eventual error? We'd know if that data was included as metadata while logging the error.

Either of these usecases would require data access in the application layer. Let me know if you have any questions.

---

_Comment by @epage on 2022-05-11 14:30_

I had been wondering if we should offer look up functions that panic instead of returning bad type errors.

To allow what @AndreasBackx brought up, we either need
- Allow people to query every known data type for an argument without panicing
- Store the "raw" version of arguments
  - This might get messy if we are able to support untyped defaults as there won't be a raw version.

---

_Comment by @epage on 2022-05-13 00:21_

So my current implementation offers a `get_raw` (on top of `get_one` and `get_many`).  At times I am tempted to remove it but at least for the current implementation, its actually a help to track the raw values.

Progress
- [x] Replace `allow_invalid_utf8` with `ValueParser`, internally
- [x] Allow users to set `ValueParser` on an `Arg` and look up the typed and raw values
- [x] Provide alternative boolean `ValueParser`s that handle env-like truthiness
- [x] Support for an `ArgEnum` `ValueParser`
- [x] Generalize possible values so bools can specify them to help with completion support (will require native bool support)
- [x] Document why types are natively supported
- [x] Support a possible-values `ValueParser`
- [x] Explore replacing `forbid_empty_value` with a `ValueParser`
- [x] Numeric range support (maybe use this as the basis for native numbers?).
- [ ] Deprecate `possible_values` in favor of `value_parser`
- [ ] Deprecate `allow_invalid_utf8` in favor of `ValueParser`
- [ ] Deprecate arg value validators in favor of `ValueParser`
- [ ] Deprecate `forbid_empty_values`
- [ ] Add `default_value_raw` / `default_values_raw` etc and deprecate the current forms

Deferred work
- Switch subcommands to `ValueParser` and away from `allow_invalid_utf8_for_external_subcommands`
- `clap_derive` support (fixing #3589) because we need a way to get values out of `ArgMatches` and it'd be a breaking change to require `Clone` and a breaking change for `FromArgMatches` to accept `&mut ArgMatches` so we can remove the already-allocated items
  - I took another approach of having `_mut` variants of the `FromArgMatches` functions that have inherent implementations to maintain compatibility but the `update` version with subcommands processes subcommands then if it fails, it falls back to `from_arg_matches` on the original matcher, so removing the sub matches breaks it

---

_Comment by @epage on 2022-05-13 18:34_

I was considering moving `ignore_case` into the `ValueParser` as each type that it matters for, like `ArgEnumValueParser` could implement it and it could be left off all the others.

However, `required_if_eq` and friends complicate this because they use `ignore_case` and I don't want to move it from `Arg` to every `ValueParser`, so for now, I'm leaving it on the `Arg`.  Maybe as we move special required rules out, we can re-evaluate this.

---

_Comment by @epage on 2022-05-16 20:45_

Currently, clap allows you accessing all of the values for a group.  The new implementation limits that to groups where all present values are of the same type.  For more background on why this was added, see https://github.com/clap-rs/clap/pull/283

---

_Referenced in [clap-rs/clap#3732](../../clap-rs/clap/pulls/3732.md) on 2022-05-17 21:47_

---

_Closed by @epage on 2022-05-18 00:15_

---

_Comment by @AndreasBackx on 2022-05-18 08:32_

@epage I like the new API, do you think this would allow for some way to implement #1206? Happy to make a PR as well, let me know if you had something in mind.

---

_Comment by @epage on 2022-05-18 11:34_

That is still blocked on another issue; I've updated the issue description to summarize what it is blocked on

---

_Referenced in [clap-rs/clap#3792](../../clap-rs/clap/issues/3792.md) on 2022-06-06 14:23_

---

_Referenced in [clap-rs/clap#5156](../../clap-rs/clap/issues/5156.md) on 2023-10-02 17:47_

---
