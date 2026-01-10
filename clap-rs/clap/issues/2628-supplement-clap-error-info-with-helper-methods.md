---
number: 2628
title: "Supplement clap::Error.info with helper methods"
type: issue
state: closed
author: TheDaemoness
labels:
  - C-enhancement
  - M-breaking-change
  - A-help
  - S-waiting-on-mentor
assignees: []
created_at: 2021-07-27T00:09:23Z
updated_at: 2022-02-07T21:37:28Z
url: https://github.com/clap-rs/clap/issues/2628
synced_at: 2026-01-10T01:27:22Z
---

# Supplement clap::Error.info with helper methods

---

_Issue opened by @TheDaemoness on 2021-07-27 00:09_

Maintainer's notes
- Ideally we'd make dynamic member access the API for this but that is still going through the RFC process
- If we want a jump start on that API, we could provide our own "any map" that allows attaching arbitrary enums to the error struct.
- Overall, this avoids limiting ourselves to each error kind having an exact set of data in every case
---
### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-beta.2

### Describe your use case

Preface: thank you very much for your work on clap so far, it has saved me countless hours of tedium.

`ErrorKind` is a fieldless enum, so additional contextual information required for someone to provide their own custom error messages is provided as a `Vec<String>` in `Error`, named `info`.

Using a `Vec` to carry this information not self-explanatory in regards to what each element of the `Vec` represents (e.g. which element represents the invalid value that triggers an `InvalidValue` or `ValueValidation`). This would be okay-ish if clap had any of the following:
* Methods or associated functions on `Error` or `ErrorKind` to retrieve specific information
* Functions, named constants, or an enum that can be used with `info` to retrieve specific information based on where it should be in the vector.
* Documentation describing what the values of `info` will be for a given `ErrorKind`.

As far as I can tell, none of these are present, which means that in order to write one's own error reporting, one needs to either [read the source code of the functions that construct these `Error`s](https://github.com/clap-rs/clap/blob/master/src/parse/errors.rs#L509) or do their own testing. I don't think either should be necessary.

### Describe the solution you'd like

The solution I'd prefer the most is a significant breaking change (described under Alternatives). I acknowledge that's not desirable, even considering the upcoming `3.0.0` release.

A compromise is to add ways to retrieve this information safely, such as by adding methods to `Error` that return the relevant information. A few possible examples with placeholder names:
* `pub fn get_arg_or_subcmd(&self) -> Option<&str>`
* `pub fn get_missing(&self) -> &[String]`
* `pub fn get_value_counts(&self) -> Option<(usize,usize)> //Found, expected`.

Implementing each method should be fairly simple, if perhaps a bit repetitive considering the number of variants on `ErrorKind` that need to be matched. The bodies would mostly be pattern matches followed by accesses to `info`.

One caveat in the implementation is that `Error` should contain a private flag to indicate whether the `Error` was constructed by `Error::with_description` or not. If so, these functions should return empty values.

### Alternatives, if applicable

This problem can technically be solved by improving documentation, but I'm of the mind that doing only that is the worst solution. `info` being a Vec is fragile and typo-prone, and there should be better ways to interact with it than as a `Vec`.

Ideally, keeping `ErrorKind` as a fieldless enum, `info`'s type would change to an enum with fields that store the relevant information for each kind of error. Some of the methods described above could still be implemented for convenience.

However, I acknowledge that this would be a significant breaking change that would take more than a few renames for users of clap to fix. Solutions like making `info` a method that generates a `Vec<String>` from data in this enum or providing both `info` and the enum in another field aren't entirely compelling either.

### Additional Context

I am very willing to work with you fine folks to implement any of these changes, if approved. Have a good day!

---

_Label `T: new feature` added by @TheDaemoness on 2021-07-27 00:09_

---

_Renamed from "Supplement crate::Error.info with helper methods" to "Supplement clap::Error.info with helper methods" by @TheDaemoness on 2021-07-27 02:13_

---

_Added to milestone `3.0` by @pksunkara on 2021-08-11 15:22_

---

_Label `C: errors` added by @pksunkara on 2021-08-11 15:22_

---

_Comment by @pksunkara on 2021-08-11 18:56_

@epage Do you think you can take care of this after the utf8 thing you are currently working on?

---

_Comment by @pksunkara on 2021-08-11 18:58_

If we are breaking it, I would actually love the variants in `ErrorKind` to be tuples of fields that can contain the metadata of the error.

---

_Comment by @epage on 2021-08-11 19:04_

After iterating with different error designs, I think our approach of a tag-only Kind with auxiliary data is the best approach (granted, how we expose that auxiliary data can be improved).  The problem with enum fields is its brittle (can't add fields without breaking compat) and harder to check for just the kind (`matches!` helps some but not completely).

The error working group is working on a general API for querying information from errors.  Something like that seems like the best long-term solution.  Short term, we can add our own query methods if we really need this before the Error WG gets their feature landed.

---

_Comment by @pksunkara on 2021-08-11 19:06_

> Error WG gets their feature landed.

Which feature is that?

> we can add our own query methods

In that case, shall I mark this as non 3.0 since it wont be breaking change?

---

_Comment by @epage on 2021-08-11 19:13_

> > Error WG gets their feature landed.
> Which feature is that?

Dynamic member access, https://github.com/rust-lang/rfcs/pull/2895

The RFC is old but it looks like work has restarted on this.  Just today they were talking about this on their Zulip channel.

> In that case, shall I mark this as non 3.0 since it wont be breaking change?

Agreed

---

_Removed from milestone `3.0` by @pksunkara on 2021-08-11 19:14_

---

_Referenced in [clap-rs/clap#2827](../../clap-rs/clap/pulls/2827.md) on 2021-10-07 16:35_

---

_Referenced in [epage/clapng#193](../../epage/clapng/issues/193.md) on 2021-12-06 21:18_

---

_Label `C: errors` removed by @epage on 2021-12-08 20:24_

---

_Label `A-help` added by @epage on 2021-12-08 20:24_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:10_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:10_

---

_Label `M-breaking-change` added by @epage on 2021-12-13 17:03_

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-13 17:07_

---

_Added to milestone `4.0` by @epage on 2021-12-14 15:25_

---

_Referenced in [clap-rs/clap#1365](../../clap-rs/clap/issues/1365.md) on 2022-02-01 17:25_

---

_Referenced in [clap-rs/clap#3227](../../clap-rs/clap/issues/3227.md) on 2022-02-02 20:04_

---

_Referenced in [clap-rs/clap#3395](../../clap-rs/clap/pulls/3395.md) on 2022-02-02 21:49_

---

_Referenced in [clap-rs/clap#3402](../../clap-rs/clap/pulls/3402.md) on 2022-02-04 18:27_

---

_Comment by @epage on 2022-02-04 18:27_

Mind taking a look at #3402 to see if it'd work?

---

_Closed by @epage on 2022-02-07 21:37_

---

_Referenced in [clap-rs/clap#3444](../../clap-rs/clap/pulls/3444.md) on 2022-02-11 01:33_

---
