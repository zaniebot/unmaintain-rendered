```yaml
number: 4395
title: Implement a partial parse method for Command
type: issue
state: open
author: 0xForerunner
labels:
  - C-enhancement
  - A-parsing
  - S-triage
assignees: []
created_at: 2022-10-17T15:50:13Z
updated_at: 2022-11-08T04:28:01Z
url: https://github.com/clap-rs/clap/issues/4395
synced_at: 2026-01-12T16:14:16Z
```

# Implement a partial parse method for Command

---

_@0xForerunner_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.0.8

### Describe your use case

I want to build an interactive parser. Imagine you have a program with dozens of nested subcommands and args. In this case it is not practical for the user to enter everything in one line. I think an interactive parser itself might be outside of scope for clap, but a method that allows partial parsing would make this much easier and more elegant for perhaps another crate to use. 
Example:
```
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema, Parser)]
#[serde(rename_all = "snake_case")]
pub enum Msg {
    SetConfig {
        #[command(subcommand)]
        set_config_msg: AnotherEnum,
    },
    AddAsset {
        asset_info_type: MyStruct,
    },
}

#[derive(Parser)]
pub struct AnotherEnum {
    Variant0,
    Variant1(MyStruct),
}

pub struct MyStruct {
    string0: String,
    string1: String
} 

```
My program would eventually look like this:

```
>>> my-cli
Enter Msg (set_config or add_asset)
>>> set_config
Enter set_config_msg (Variant0 or Variant1)
>>> Variant1
Enter string0:
>>> Hello
Enter string1:
>>> World
Done
```

### Describe the solution you'd like

I think there are a couple things that need to be built to allow this. To reiterate I don't think clap should be doing the readline implementation, just the partial parsing.
I imagine the function signature would looks something like this
```
    pub fn try_get_partial_matches_from<I, T>(mut self, itr: I) -> ClapResult<PartialArgMatches>
    where
        I: IntoIterator<Item = T>,
        T: Into<OsString> + Clone,
    {
        ...
    }
```
and PartialArgMatches would contain all the same information, except potentially omit any info from the next subcommand:
```
#[derive(Debug, Clone, Default, PartialEq, Eq)]
pub struct PartialArgMatches {
    #[cfg(debug_assertions)]
    pub(crate) valid_args: Vec<Id>,
    #[cfg(debug_assertions)]
    pub(crate) valid_subcommands: Vec<Str>,
    pub(crate) args: FlatMap<Id, MatchedArg>,
    pub(crate) subcommand: Option<Box<PartialSubCommand>>,
}

#[derive(Debug, Clone, PartialEq, Eq)]
pub(crate) struct PartialSubCommand {
    pub(crate) name: String,
    pub(crate) matches: Result<PartialArgMatches, MatchesError>,
}
```
try_get_partial_matches_from would succeed if it receives all required args and required subcommands for the specific command it was called on. It would attempt to traverse down the subcommand tree, but if it fails it would simply return as much successful parsing as possible, and the error would be contained in a PartialSubCommand

I don't think this will be too much work hopefully, as the new method should be very similar to what is already created. The only difference really is when to error. Ideally try_get_partial_matches_from will only error if it cannot successfully parse the very first subcommand.



---

_Label `C-enhancement` added by @0xForerunner on 2022-10-17 15:50_

---

_Comment by @epage on 2022-10-17 16:56_

At first I thought you were discussing a REPL which [clap already supports](https://docs.rs/clap/latest/clap/_derive/_cookbook/repl/index.html) but instead you are wanting to prompt the user for each part of the struct as it gets built up.

We do have #1634 for prompts for fields, which would act as another input like an arg, an env variable, a default, etc. 

But your use case goes further and has people entering sub-prompts for subcommands.

I don't think this is as simple as the issue describes it to be to implement as this is effectively pausing the parser and having it resumed later.  This also feels too specialized that the work and code to support this wouldn't give enough pay off.  This would also not work with the derive which the introductory code makes it sound like it would be expected.

On reddit, I had brought up the idea of CommandActions like ArgActions but seeing more detail, I'm unsure if that would apply; it'd very much depend on how we design the actions.

---

_Comment by @0xForerunner on 2022-10-17 17:34_

@epage thanks so much for taking the time to look at this. I dug a little deeper into clap this morning and I agree it looks like it might be more complicated than I suggested above. I'll look into those link you sent and see if perhaps that will work as a workaround for me. The CommandActions that I was looking for on reddit would have been a hacky workaround to get the functionality that I want, but the feature request above is really what I'm looking to achieve. I'm curious If you have any other ideas for how I can achieve this?

---

_Comment by @0xForerunner on 2022-10-17 18:01_

> This would also not work with the derive which the introductory code makes it sound like it would be expected.

Why would this not work with derive? There might be something I'm not understanding here.



---

_Comment by @0xForerunner on 2022-10-17 18:05_

@epage  If I were to do this work myself, do you imagine that this could potentially be included in clap via a PR? or do you think it's too niche to warrant inclusion within clap? If you think it could eventually be included, could you perhaps give me some guidance on how you would implement it? I don't have any experience with clap so getting a little insight would be great.

---

_Comment by @epage on 2022-10-20 01:50_

> Why would this not work with derive? There might be something I'm not understanding here.

The derive needs everything parsed to be able to fill in the fields.

> @epage If I were to do this work myself, do you imagine that this could potentially be included in clap via a PR? or do you think it's too niche to warrant inclusion within clap? If you think it could eventually be included, could you perhaps give me some guidance on how you would implement it? I don't have any experience with clap so getting a little insight would be great.

Native, built-in support for this exact work flow is unlikely to get in unless we see a wider need.  If we can find a solution that provides the building blocks for your need that can be useful more broadly, then that can work.  For example, if we could expose a single parse pass and callers build on top of that, then that might work.

---

_Comment by @0xForerunner on 2022-10-21 04:23_

That sounds good @epage. I created a super basic repo called [clap-interactive](https://github.com/ewoolsey/clap-interactive) with support for clap. I'm not sure that's the best way to get the functionality that I want but it works pretty well for what I need. I am still looking for a better way to begin the interactive mode when the clap parser errors out.

---

_Label `S-triage` added by @epage on 2022-11-08 04:27_

---

_Label `A-parsing` added by @epage on 2022-11-08 04:28_

---

_Referenced in [clap-rs/clap#6185](../../clap-rs/clap/issues/6185.md) on 2025-11-18 16:30_

---
