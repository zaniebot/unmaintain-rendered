---
number: 2840
title: Deprecate cargo macros
type: issue
state: open
author: kbknapp
labels:
  - C-bug
  - A-builder
  - E-easy
assignees: []
created_at: 2021-10-09T17:32:37Z
updated_at: 2022-06-08T16:03:53Z
url: https://github.com/clap-rs/clap/issues/2840
synced_at: 2026-01-07T13:12:19-06:00
---

# Deprecate cargo macros

---

_Issue opened by @kbknapp on 2021-10-09 17:32_

Maintainer's notes

Path forward
1. Create a crate with these, and other related cargo env variables
2. Port clap over to this crate
3. Deprecate the `crate_*` macros, pointing people to that crate
---
### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-beta.4

### Describe your use case

I'm proposing removing the convenience macros for getting information from `cargo` at build time. Specifically:

- `crate_license`
- `crate_version`
- `crate_authors`
- `crate_description`
- `crate_name`

None of these utilize anything inside of `clap` and simply call out to checking environment variables that `cargo` populates at compile time.



### Describe the solution you'd like

These would be prime candidates for spinning off into a crate dedicated to such information about Cargo. I'd actually be kind of surprised if a crate with this functionality doesn't already exist. If one doesn't exist, I'm not against hosting one inside the clap-rs org, but being that it's not actually clap related this repository is probably a bad fit.

I propose we deprecate for v3, and remove for v4. That gives us time to either find a crate to suggest or pull these macros into one ourselves.


### Alternatives, if applicable

- We keep these macros unchanged as they're small and self contained

### Additional Context

I'm open to suggestions and thoughts on the matter.

---

_Label `T: RFC / question` added by @kbknapp on 2021-10-09 17:32_

---

_Label `E: breaking change` added by @kbknapp on 2021-10-09 17:32_

---

_Added to milestone `3.0` by @kbknapp on 2021-10-09 17:32_

---

_Comment by @pksunkara on 2021-10-09 18:02_

I would agree but we use them in `app_from_crate!` and as far as I understand from looking at people creating issues about this part of the code these are really widely used for most the non-popular clis.

---

_Comment by @kbknapp on 2021-10-09 18:55_

 `app_from_crate!` could use whatever crate these would get spun off into so I'm not too concerned from that angle.

I'm more worried that because these aren't clap related, what if people want more cargo metadata exposed? These started as just `crate_version!` but have since grown to a few more. Sure they're not big or complex, but I think they'd fit better into a crate dedicated to that functionality. 

I also agree they're pretty widely used which I think adds support to us taking it slow *if* we move forward (deprecate for a while, remove in a future major release), and also adds support to us creating the crate in our org with an identical API to make the transition painless.

---

_Renamed from "Remove / Deprecate cargo macros" to "Deprecate cargo macros" by @pksunkara on 2021-10-09 19:01_

---

_Label `T: RFC / question` removed by @pksunkara on 2021-10-09 19:01_

---

_Label `E: breaking change` removed by @pksunkara on 2021-10-09 19:01_

---

_Label `C: other` added by @pksunkara on 2021-10-09 19:01_

---

_Comment by @epage on 2021-10-09 19:15_

I can understand the desire to spin things off.  I've wanted `arg_enum!` in non-clap settings.

I agree that the right route forward is a crate we delegate to but still provide a convenience for clap users.

---

_Comment by @epage on 2021-10-09 19:15_

Since a deprecation is not a breaking change, should we move this out of the 3.0 milestone?

---

_Comment by @pksunkara on 2021-10-09 19:25_

It is in 3.0 milestone similar to #2835 because we need to add deprecation warnings.

---

_Comment by @epage on 2021-10-09 19:29_

We should not be deprecating anything without having a replacement to point people towards.  Unless we are creating the crate now, we shouldn't do this.

With #2835, we have replacements (builder, derive, from usage) that we can point people to as the recommended approach.

---

_Comment by @kbknapp on 2021-10-10 01:08_

> Since a deprecation is not a breaking change, should we move this out of the 3.0 milestone?

Good point. My point was just to deprecate for 3.0 like @pksunkara mentioned, but you're correct we could deprecate anywhere in the 3.x lifetime. I'll place this at the 3.1 milestone as a placeholder for "decide on this sometime in 3.x" 

And of course we're not locked into spinning these out, I'm open to suggestions and opinions on the matter too.


---

_Removed from milestone `3.0` by @kbknapp on 2021-10-10 01:08_

---

_Added to milestone `3.1` by @kbknapp on 2021-10-10 01:08_

---

_Comment by @joshtriplett on 2021-10-11 09:29_

I think it'd be perfectly reasonable to just make these non-public before 3.0, as long as the top-level app_from_crate macro still exists.

I don't think we have to provide a replacement first. These macros just existed to pass to the appropriate clap functions.

---

_Comment by @epage on 2021-10-11 14:05_

> I think it'd be perfectly reasonable to just make these non-public before 3.0, as long as the top-level app_from_crate macro still exists.

For context, I'm recommending in https://github.com/clap-rs/clap/issues/2617 that we provide built-in transition paths for users via deprecations to streamline the upgrade process.  We can do breaking changes but ideally we have a deprecation windows first

---

_Comment by @pksunkara on 2021-10-11 14:10_

> I think it'd be perfectly reasonable to just make these non-public before 3.0, as long as the top-level app_from_crate macro still exists.

IIRC, people actually use those underlying macros directly instead of `app_from_crate!` and created some issues around them.

---

_Referenced in [epage/clapng#213](../../epage/clapng/issues/213.md) on 2021-12-06 22:17_

---

_Label `C: other` removed by @epage on 2021-12-08 20:18_

---

_Label `A-builder` added by @epage on 2021-12-08 20:18_

---

_Label `C-bug` added by @epage on 2021-12-13 21:23_

---

_Label `E-easy` added by @epage on 2021-12-13 21:23_

---

_Comment by @epage on 2022-02-02 19:48_

Random notes:
- I recommend the `crate-ci` org for this
- We should cross-link with [build_script](https://github.com/ALinuxPerson/build_script) and [shadow](https://crates.io/crates/shadow-rs) for other cargo related APIs
- `build-env` and `cargo-env` names are taken.  The next best I can think of is `pkg-env`.

---

_Referenced in [baoyachi/shadow-rs#69](../../baoyachi/shadow-rs/issues/69.md) on 2022-02-22 02:35_

---

_Removed from milestone `3.x` by @epage on 2022-06-08 16:03_

---

_Added to milestone `4.x` by @epage on 2022-06-08 16:03_

---

_Referenced in [clap-rs/clap#4514](../../clap-rs/clap/pulls/4514.md) on 2022-11-27 16:16_

---
