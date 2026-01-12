```yaml
number: 2717
title: Deprecate AppSettings / ArgSettings in favor of binary methods
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - M-breaking-change
  - A-builder
  - S-waiting-on-design
assignees: []
created_at: 2021-08-18T15:21:59Z
updated_at: 2022-07-22T20:41:45Z
url: https://github.com/clap-rs/clap/issues/2717
synced_at: 2026-01-12T16:14:13Z
```

# Deprecate AppSettings / ArgSettings in favor of binary methods

---

_@epage_

Since I was already in a conversation with kbknapp, I figured I might as well ask him for the reason for the transitio to ArgSettings early in clap3.

> In part, it was primarily the fn(bool) functions because at the time (perhaps still so?) the Rust API guidelines suggested taking a bool is not as good as an enum variant which is more clear. It was also more in line with App structs which are primarily configured via AppSettings and don't have fn(bool) methods.
>
> However, I also didn't want two ways to do certain settings (i.e. both setting(ArgSetting) and fn(bool)), so I picked enum variants for those bool-ish settings. 

The guideline in question: [Arguments convey meaning through types, not bool or Option (C-CUSTOM-TYPE)](https://rust-lang.github.io/api-guidelines/type-safety.html#c-custom-type)

The motivations given are:
- it is not immediately clear what true and false are conveying without looking up the argument names
- Using custom types makes it easier to expand the options later on, for example by adding an ExtraLarge variant.

This motivation doesn't quite line up with clap because the arguments are being used in a builder position which automatically names things.  Its fairly common in builders to still use bools.  Its also hard to predict what the expansion should be for what to name enum variants.  Plus, they would balloon the API.

## Proposal

We emphasize the methods, restoring any inferring, and work to deprecate the `ArgSettings` / `AppSettings`.  Besides the improvements below, this will make the clap2->clap3 transition easier

This is only affecting the API and is independent of how we store these settings.

## Problems with `Settings`

User convenience of inferring settings

Unnecessary verbosity: `#[clap(multiple_occurrences = true)]` vs `#[clap(setting(clap::ArgSettings::MultipleOccurrences)]`

Interferes with actually following the API guidelines (see the individual `Color` settings vs having an enum

Disjoint API leaking implementation details: Related to the above, we have to distinct ways of doing things, based on the conceptual data type.  This makes the API more confusing and increases the barrier to change, including to follow the guidelines this was done for (again, see above with Color)

Keeping both means we are maintaining two distinct APIs for modifying the builders

For global settings, we can "layered" the Apps: for opted-in settings, we track whether they have been explicitly set and for anything not explicitly set, we automatically propagate the parent Apps settings.  See https://github.com/clap-rs/clap/issues/1184 for more details.

## kbknapp's thoughts

While kbknapp's word isn't the only consideration here, I thought I'd add his thoughts

Regarding `Settings`

> This has one bigger downside of you can no longer easily abstract over a CLI setting dynamically, such as
>
> ```
> let should_be_required: bool = /*.. some method of determining this outside of clap */; arg.required(should_be_required);
> ```
> But I think that's a rare enough use case that if one wanted they could manually unset_setting.

> Another downside, or at least it could be argued as a downside, is for documenting, functions are better than enum variants. But this to me is somewhat moot, as there will always been some settings as ArgSettings and some as fn(...) at least non-bool methods.


> As far pure keyboard typing goes, the methods are easier to type and subjectively to me more visually pleasing than the settting(Foo). 

As for which direction to go, he originally stated:

> I'm not sold either way though and could be swayed as to which method is "better"

and later

> I agree with everything you said. To be clear, I'm definitely not against the bools and part of the reason I pulled back a little on it.

_Originally posted by @epage in https://github.com/clap-rs/clap/discussions/2607#discussioncomment-1151763_

---

_Renamed from "Migrate from ArgSettings to binary methods" to "Deprecate AppSettings / ArgSettings in favor of binary methods" by @epage on 2021-08-18 15:25_

---

_Comment by @epage on 2021-08-18 15:25_

As we resolve this, we'll need to create issues for
- Removing the Settings in a future major release
- Migrate color control to an enum, from distinct binary flags
  - https://github.com/clap-rs/clap/issues/2811

---

_Added to milestone `4.0` by @pksunkara on 2021-08-18 15:30_

---

_Comment by @epage on 2021-08-18 15:39_

@pksunkara I think this should be tagged for the 3.0 release as it will lower the barrier for transition from clap2->3.  Note that this is only for deprecating and not removing settings.  Removing the settings would definitely for a 4.0 thing

EDIT: And will resolve documentation regressions

---

_Label `C: settings` added by @pksunkara on 2021-08-31 11:58_

---

_Comment by @pksunkara on 2021-09-23 11:27_

> However, I also didn't want two ways to do certain settings (i.e. both setting(ArgSetting) and fn(bool)), so I picked enum variants for those bool-ish settings.

To confirm, the settings approach was picked instead of the methods approach first. But then, we started adding convenience helper methods (Ex: `require_delimiter` in 920b5595ed72abfb501ce054ab536067d8df2a66 and 09d4d0a9038d7ce2df55c2aec95e16f36189fcee). As you can see from that code, these helper methods are not always simply toggling the setting, but rather toggling one or more related settings when the method is called.

Basically, the methods we added are builder methods, but they are not 1:1 with the settings. They are helper methods which manipulate one or more settings.

> Early in clap3 history, it looks like the plan was to deprecate binary methods on args (e.g. `arg.takes_value(true)`) in favor of arg settings (e.g. `arg.setting(ArgSettings::TakesValue)`) in clap2 with them removed in clap3 (see 6fc70d825 and #1037).  This was reverted in 82ffb821d to ease the transition.

We had to revert the removing of methods because of the above reason. They are helpers which change one or more settings and I don't think kbknapp realised that.

> Interferes with actually following the API guidelines (see the individual Color settings vs having an enum

Color settings should never have been there in the first place. I agree that it should be a separate enum and should be used by a method which takes that enum. Let's fix this for 3.0 by creating an issue **"Migrate color control to an enum, from distinct binary flags"**

> Disjoint API leaking implementation details: Related to the above, we have to distinct ways of doing things, based on the conceptual data type. This makes the API more confusing and increases the barrier to change, including to follow the guidelines this was done for (again, see above with Color)

I am not sure what exactly you mean by this, but I assume it's an issue with the color related settings only. Thus, this would be fixed by removing them as proposed above.

> We emphasize the methods, restoring any inferring, and work to deprecate the `ArgSettings` / `AppSettings`. Besides the improvements below, this will make the clap2->clap3 transition easier

I would argue that the transition is harder compared to the reverse.

* `App` doesn't have any of those methods and all the settings are currently being done by `*_setting()`
* `Arg`'s helper methods are converted to binary methods already so they don't exactly maintain backward compatibility.
  - [old `require_equals`][] -> [new `require_equals`][]
  - [old `require_delimiter`][] -> [new `require_delimiter`][]
  - [old `empty_values`][] -> [new `forbid_empty_values`][]

> Keeping both means we are maintaining two distinct APIs for modifying the builders

> I'm not sold either way though and could be swayed as to which method is "better"

What I would propose instead is to remove *(Deprecation discussion in #2617)* the binary methods we currently have for `Arg` and go with settings enum everywhere (examples, tests, docs etc..).

[old `require_equals`]: https://github.com/clap-rs/clap/blob/v2.33.3/src/args/arg.rs#L820
[new `require_equals`]: https://github.com/clap-rs/clap/blob/5580e8c4657dea6ac085ce76102c3aefe5f33d81/src/build/arg/mod.rs#L3622
[old `require_delimiter`]: https://github.com/clap-rs/clap/blob/v2.33.3/src/args/arg.rs#L2992
[new `require_delimiter`]: https://github.com/clap-rs/clap/blob/5580e8c4657dea6ac085ce76102c3aefe5f33d81/src/build/arg/mod.rs#L3754
[old `empty_values`]: https://github.com/clap-rs/clap/blob/v2.33.3/src/args/arg.rs#L2297
[new `forbid_empty_values`]: https://github.com/clap-rs/clap/blob/5580e8c4657dea6ac085ce76102c3aefe5f33d81/src/build/arg/mod.rs#L4186
[Demangle interlinking flags]: https://github.com/clap-rs/clap/pull/2362


---

_Removed from milestone `4.0` by @pksunkara on 2021-09-23 11:27_

---

_Added to milestone `3.0` by @pksunkara on 2021-09-23 11:27_

---

_Comment by @epage on 2021-09-24 15:06_

I'm a bit surprised, your response feels like a 180 but maybe I misinterpreted us moving this from discussion to Issue.  I had thought that we were past the high level discussion and good with the general direction.

Whatever the motivation was, if we still have this  much to discuss, this would have best left to discussions because of threading so we can more easily keep the topics focused.

I'm also confused by parts of your response.

> To confirm, the settings approach was picked instead of the methods approach first. But then, we started adding convenience helper methods (Ex: require_delimiter in 920b559 and 09d4d0a). As you can see from that code, these helper methods are not always simply toggling the setting, but rather toggling one or more related settings when the method is called.

> Basically, the methods we added are builder methods, but they are not 1:1 with the settings. They are helper methods which manipulate one or more settings.

Clap started only with the methods, for example [required in clap v1](https://docs.rs/clap/1.0.0/clap/struct.Arg.html#method.required)

Not digging through it all and taking what kbnapp said, the Settings were then added  in an attempt to comply with the API guidelines.

> We had to revert the removing of methods because of the above reason. They are helpers which change one or more settings and I don't think kbknapp realised that.

So maybe there were multiple reasons or different angles on the same but https://github.com/clap-rs/clap/commit/82ffb821de9b261662f8afe5d4d4d8fc29ec4c1a explicitly says the reason was to smooth the upgrade process.


Regardless of any of the above, I don't see how this impacts my proposal.  Is there something that the user can do today through Settings that they can't do through methods that is a motivating use case to focus on Settings?

> > Disjoint API leaking implementation details: Related to the above, we have to distinct ways of doing things, based on the conceptual data type. This makes the API more confusing and increases the barrier to change, including to follow the guidelines this was done for (again, see above with Color)

> I am not sure what exactly you mean by this, but I assume it's an issue with the color related settings only. Thus, this would be fixed by removing them as proposed above.

Let me re-phrase.  We have parallel APIs for building Args and Apps, settings and methods.
- Not all methods can be expressed via settings but all settings can be expressed in methods.  By supporting Settings, we are forcing the user to use both of the parallel APIs that feel foreign to each other
- Settings in the API is the low friction path and encourages developers to favor it, even if it isn't always in the best interest.  I used `Color` as an example.  Yes, we can fix color but that still leaves the environment that led to the creation of the color settings.
- The Settings API exists more as a reflection that we implement an optimization for storing a lot configuration as bit flags.  Focusing API details on implementation leads to a brittle, obtuse API.

> I would argue that the transition is harder compared to the reverse.

> - App doesn't have any of those methods and all the settings are currently being done by *_setting()

My proposal was to start with deprecating settings, not obsoleting / removing them.  Also, from my other investigations, there are generally 0-3 App Settings while there are a lot of arg methods.

> - Arg's helper methods are converted to binary methods already so they don't exactly maintain backward compatibility. 

I'm not sure what you mean by "helper" vs "binary" method and what I'm supposed to notice is different in the linked functions.

> What I would propose instead is to remove (Deprecation discussion in #2617) the binary methods we currently have for Arg and go with settings enum everywhere (examples, tests, docs etc..).

So if I'm understanding, it sounds like you are in favor of resuming kbnapp's efforts to remove the methods, in favor of settings?  Under what pretense? I did not see any justification given for them and yet there are many flaws with the settings API, as has been previously enumerated.

---

_Comment by @pksunkara on 2021-09-24 15:36_

> I'm a bit surprised, your response feels like a 180 but maybe I misinterpreted us moving this from discussion to Issue. I had thought that we were past the high level discussion and good with the general direction.

I am sorry, it was my mistake in misunderstanding. When you asked the below question in that discussion:

> Are we agreed on this for it to be worth creating an Issue so this can be tracked as part of the 4.0 milestone

I thought you wanted to discuss this more in this issue to finalize the design. I was under the assumption that we were using the discussions threads as a **maybe idea** and issues as **todo idea**. Which is why I answered yes to create an issue as an agreement that we need to resolve this.

Am I correct in assuming that you want to move the whole design finalization to discussions?

---

_Comment by @epage on 2021-09-24 16:02_

> I thought you wanted to discuss this more in this issue to finalize the design. I was under the assumption that we were using the discussions threads as a maybe idea and issues as todo idea. Which is why I answered yes to create an issue as an agreement that we need to resolve this.

> Am I correct in assuming that you want to move the whole design finalization to discussions?

I'll break this down into
- Problem
- Solution (focused on requirements, use cases, and workflows)
- Implementation (focused on code)

With each being broken down into high level and detailed.

We discussed the Problem and Solution and I thought we were agreed and ready to move on to Implementation.   However, if we are still working out Settings vs Methods, then we have not agreed on a Solution.

Where it lives relative to where we are at in the process doesn't matter too much except for how much discussion it generates. Issues being single threaded make it hard to break down and discuss individual points.

---

_Comment by @pksunkara on 2021-10-01 00:21_

> We discussed the Problem and Solution and I thought we were agreed and ready to move on to Implementation. However, if we are still working out Settings vs Methods, then we have not agreed on a Solution.

Yeah, we haven't agreed on a solution. As I said, it was a miscommunication that gave the impression that we did.

---

_Referenced in [clap-rs/clap#3028](../../clap-rs/clap/issues/3028.md) on 2021-11-15 18:08_

---

_Referenced in [epage/clapng#200](../../epage/clapng/issues/200.md) on 2021-12-06 21:20_

---

_Comment by @pksunkara on 2021-12-07 02:59_

Proposing to punt this to v4 to get 3.0.0-rc.0 out this week.

---

_Removed from milestone `3.0` by @pksunkara on 2021-12-07 17:36_

---

_Added to milestone `4.0` by @pksunkara on 2021-12-07 17:36_

---

_Label `C: settings` removed by @epage on 2021-12-08 20:17_

---

_Label `A-builder` added by @epage on 2021-12-08 20:17_

---

_Referenced in [clap-rs/clap#2513](../../clap-rs/clap/issues/2513.md) on 2021-12-13 16:46_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-13 17:12_

---

_Label `C-enhancement` added by @epage on 2021-12-13 17:13_

---

_Label `M-breaking-change` added by @epage on 2021-12-13 17:13_

---

_Referenced in [clap-rs/clap#3002](../../clap-rs/clap/issues/3002.md) on 2022-01-10 14:54_

---

_Referenced in [aobatact/clap-serde#7](../../aobatact/clap-serde/issues/7.md) on 2022-01-21 15:38_

---

_Comment by @epage on 2022-02-09 01:16_

In switching to functions, we use global vs non-global.  Overall I think this can help because its easy to overlook this and do the wrong thing.  Granted, there are cases where things might go either way.

My long term thought for addressing replacing this system is hard coding whether an attribute is "inheritable" or not.  We then layer `App`s on top of each other.  Inheritable fields will be tri-state (similar to `Option<bool>`).  If the current `App` has any fields that are like `None`, then we'll inherit the parent `App`s value.  Short term, the inheritable functions call their global version.

One challenge will be in documenting this.

Now to break down which should be inheritable.

Inheritable (global)
- DisableColoredHelp: uniform look
- DisableHelpFlag: whatever the user is doing, they are probably doing it everywhere
- DisableHelpSubcommand: whatever the user is doing, they are probably doing it everywhere
- DisableVersionFlag: whatever the user is doing, they are probably doing it everywhere
- DontCollapseArgsInUsage: uniform look
- HelpExpected: uniform assertions
- HidePossibleValues: If we're setting it at this wide of a scope then its probably wanted in the whole hierarchy
- NextLineHelp: uniform look
- PropagateVersion: uniform behavior
- InferLongArgs: uniform behavior
- InferSubcommands: uniform behavior
- AllArgsOverrideSelf: uniform behavior
- DontDelimitTrailingValues: uniform behavior
- IgnoreErrors: whatever the user is doing, they are probably doing it everywhere

Non-inheritable (non-global)
- AllowHyphenValues: This seems context-specific
- AllowNegativeNumbers
- ArgRequiredElseHelp: This implies required args which is context specific
- AllowMissingPositional: This seems context-specific (how you've arranged the current positionals)
- TrailingVarArg: This seems context-specific
- AllowExternalSubcommands: This is context-specific
  - AllowInvalidUtf8ForExternalSubcommands
- Hidden: This is referring to the current subcommand
- Multicall: This is only useful for top-level App
- NoBinaryName: This is only useful for top-level App
- SubcommandRequired: This is context-specific
- ArgsNegateSubcommands: This seems context-specific
- SubcommandPrecedenceOverArg: This seems context-specific
- SubcommandsNegateReqs: This seems context-specific

Removing in 4.0
- DeriveDisplayOrder: https://github.com/clap-rs/clap/issues/2808
- NoAutoHelp: https://github.com/clap-rs/clap/issues/3405
- NoAutoVersion: https://github.com/clap-rs/clap/issues/3405
- WaitOnError: https://github.com/clap-rs/clap/issues/3439
- UseLongFormatForHelpSubcommand: https://github.com/clap-rs/clap/issues/3440
- SubcommandRequiredElseHelp: #3456 

---

_Referenced in [clap-rs/clap#3434](../../clap-rs/clap/pulls/3434.md) on 2022-02-10 16:09_

---

_Referenced in [clap-rs/clap#3438](../../clap-rs/clap/pulls/3438.md) on 2022-02-10 17:18_

---

_Referenced in [clap-rs/clap#3444](../../clap-rs/clap/pulls/3444.md) on 2022-02-11 01:33_

---

_Referenced in [clap-rs/clap#3449](../../clap-rs/clap/pulls/3449.md) on 2022-02-11 18:25_

---

_Referenced in [clap-rs/clap#3450](../../clap-rs/clap/issues/3450.md) on 2022-02-11 18:34_

---

_Comment by @epage on 2022-02-11 20:17_

Remaining work, exclusive for 4.0, is the layering of the global settings.

---

_Referenced in [clap-rs/clap#3472](../../clap-rs/clap/pulls/3472.md) on 2022-02-15 13:27_

---

_Referenced in [clap-rs/clap#3495](../../clap-rs/clap/issues/3495.md) on 2022-02-22 03:21_

---

_Referenced in [clap-rs/clap#3800](../../clap-rs/clap/pulls/3800.md) on 2022-06-08 17:08_

---

_Comment by @epage on 2022-07-22 20:41_

Since no one has reported a problem where layering is needed, I'm going to close this issue out.  Layering can be added in a later breaking release and tracked in its own issue.

---

_Closed by @epage on 2022-07-22 20:41_

---
