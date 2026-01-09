---
number: 2005
title: Use enum as subcommands in a subcommand
type: issue
state: closed
author: leotaku
labels:
  - C-bug
  - E-hard
  - A-derive
assignees: []
created_at: 2020-07-09T01:30:18Z
updated_at: 2021-07-16T20:39:06Z
url: https://github.com/clap-rs/clap/issues/2005
synced_at: 2026-01-07T13:12:19-06:00
---

# Use enum as subcommands in a subcommand

---

_Issue opened by @leotaku on 2020-07-09 01:30_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closes issues

### Code

```rust
use clap::Clap;

#[derive(Clap)]
enum App {
    Build,
    Config(Config),
}

#[derive(Clap)]
enum Config {
    Show,
    Edit,
}

fn main() {
    App::parse_from(&["app", "config"]);
}
```

#### Output
```
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', src/main.rs:9:10
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

### Steps to reproduce the issue
1. Run `cargo run --`
2. Observe that the application panics

### Version

* **Rust**: rustc 1.46.0-nightly (0c03aee8b 2020-07-05)
* **Clap**: 3.0.0-beta.1

### Actual Behavior Summary

When deriving `Clap` for enums as shown in the code, calling any of the `parse` methods while not giving a secondary subcommand to primary subcommand causes a panic.

### Expected Behavior Summary

I think that instead of panicking, clap should return an error displaying the help message.

### Additional context

I have already opened a discussion about this at #2004, because I was unsure if this behavior was unexpected.
@CreepySkeleton agrees that this is a bug.

### Debug output
<details>
<summary> Debug Output </summary>
<pre>
<code>
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_derive_display_order:clap-test
[            clap::build::app] 	App::_derive_display_order:build
[            clap::build::app] 	App::_derive_display_order:config
[            clap::build::app] 	App::_derive_display_order:show
[            clap::build::app] 	App::_derive_display_order:edit
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[            clap::build::app] 	App::_create_help_and_version: Building help
[            clap::build::app] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:help
[            clap::build::arg] 	Arg::_debug_asserts:version
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing '"config"' ([99, 111, 110, 102, 105, 103])
[         clap::parse::parser] 	Parser::is_new_arg: "config":NotFound
[         clap::parse::parser] 	Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser] 	Parser::is_new_arg: probably value
[         clap::parse::parser] 	Parser::is_new_arg: starts_new_arg=false
[         clap::parse::parser] 	Parser::possible_subcommand: arg="config"
[         clap::parse::parser] 	Parser::get_matches_with: possible_sc=true, sc=Some("config")
[         clap::parse::parser] 	Parser::parse_subcommand
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[            clap::build::app] 	App::_propagate:clap-test
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_derive_display_order:config
[            clap::build::app] 	App::_derive_display_order:show
[            clap::build::app] 	App::_derive_display_order:edit
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[            clap::build::app] 	App::_create_help_and_version: Building help
[            clap::build::app] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:help
[            clap::build::arg] 	Arg::_debug_asserts:version
[         clap::parse::parser] 	Parser::parse_subcommand: About to parse sc=config
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::remove_overrides
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::gather_conflicts
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::validate_required_unless
[      clap::parse::validator] 	Validator::validate_matched_args
[         clap::parse::parser] 	Parser::remove_overrides
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::gather_conflicts
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::validate_required_unless
[      clap::parse::validator] 	Validator::validate_matched_args
[    clap::parse::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[]
</code>
</pre>
</details>


---

_Label `T: bug` added by @leotaku on 2020-07-09 01:30_

---

_Label `C: derive macros` added by @CreepySkeleton on 2020-07-09 09:10_

---

_Label `D: hard` added by @CreepySkeleton on 2020-07-09 09:10_

---

_Label `P1: urgent` added by @CreepySkeleton on 2020-07-09 09:10_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-07-09 09:10_

---

_Comment by @CreepySkeleton on 2020-07-09 10:07_

OK, got it.

Some (modified) code from discussions that works:
```rust
#[derive(Clap)]
pub struct App {
  #[clap(long = "verbose")]
  verbose: bool,
  // ...

  #[clap(subcommand)]
  subcommand: Subcommand,
}

#[derive(Clap)]
pub enum Subcommand {
  Config(Config)
  // ...
}

// THIS "SHIM" STRUCT IS MANDATORY
#[derive(Clap)]
pub struct Config {
  #[clap(subcommand)]
  subcommand: ConfigSubcommand
}

#[derive(Clap, Debug)]
pub enum ConfigSubcommand {
   Show,
   Edit,
}
```

The thing is, OP wants to omit the shim struct and use the enum directly:
```rust
#[derive(Clap)]
pub struct App {
  #[clap(long = "verbose")]
  verbose: bool,
  // ...

  #[clap(subcommand)]
  subcommand: Subcommand,
}

#[derive(Clap)]
pub enum Subcommand {
  // NO SHIMS INVOLVED
  Config(Config)
  // ...
}

#[derive(Clap)]
enum Config {
    Show,
    Edit,
}
```

That kind of makes sense. But it doesn't work because of [this](https://github.com/clap-rs/clap/pull/1681#issuecomment-584840564). I'll be working on it once I'm done with my current backlog.

---

_Comment by @pksunkara on 2020-07-11 15:05_

It used to work on structopt, right? How can we fix this?

---

_Renamed from "Parse methods panic on subcommands only derived from enums" to "Use enum as subcommands in a subcommand" by @pksunkara on 2020-07-11 15:06_

---

_Comment by @CreepySkeleton on 2020-07-11 16:35_

I believe the best way is to change the syntax:
```rust
enum Foo {
    // no attr = flatten, backcompat
    Foo(Ext),
   
    // tell derive that ExtFlat is struct and it must be flattened 
   #[clap(flatten)]
   Bar(ExtFlat),

   // tell derive that ExtSub is enum and it must be used as subcommands set
   #[clap(subcommand)]
   Bar(ExtSub),

   // synonym for current #[structopt(flatten)]
   // see https://github.com/TeXitoi/structopt/issues/327
   #[clap(subcommand(flatten))]
   Zog(ExtSubFlat)
}
```

---

_Comment by @pksunkara on 2020-07-12 09:11_

Your snippet is not exactly giving me the full context. But I can wait for the PR to review it properly.

---

_Referenced in [arlyon/holding#1](../../arlyon/holding/issues/1.md) on 2020-09-06 11:09_

---

_Referenced in [clap-rs/clap#2167](../../clap-rs/clap/issues/2167.md) on 2021-01-03 22:44_

---

_Comment by @sammko on 2021-06-07 21:23_

Something like this works for me without the extra struct
```rust
#[derive(Clap)]
pub enum Subcommand {
  // NO SHIMS INVOLVED
  Config {
    #[clap(subcommand)]
    sub: Config
  }
  // ...
}
```

---

_Assigned to @pksunkara by @pksunkara on 2021-07-09 03:00_

---

_Unassigned @pksunkara by @pksunkara on 2021-07-09 03:00_

---

_Comment by @pksunkara on 2021-07-09 03:09_

My current thinking is that we should be able to get this working without the need for any additional attributes.

---

_Comment by @epage on 2021-07-09 18:06_

Looking to catch up on this.  It sounds we had an undefined case (enum flattened into struct) and a "questionable" case (enum nested in enum) and #1681 removed. 

I say "questionable" because @CreepySkeleton raised concerns in #1681 but now we are here with a request for it to work.  @CreepySkeleton's concerns were:
- It wasn't a documented case
- Its a magical solution, involving figuring out what is on the other side.

The known/expected case for nested subcommands is to have a struct inbetween with a `#[clap(subcommand)]` on a field in the struct, whether that struct is explicit or implicit in the parent enum's definition.

#1681 also included a recommendation for handling of the attributes and that has been brought up in this thread as well:

> ```rust
> enum Foo {
>     // no attr = flatten, backcompat
>     Foo(Ext),
>    
>     // tell derive that ExtFlat is struct and it must be flattened 
>    #[clap(flatten)]
>    Bar(ExtFlat),
> 
>    // tell derive that ExtSub is enum and it must be used as subcommands set
>    #[clap(subcommand)]
>    Bar(ExtSub),
> 
>    // synonym for current #[structopt(flatten)]
>    // see https://github.com/TeXitoi/structopt/issues/327
>    #[clap(subcommand(flatten))]
>    Zog(ExtSubFlat)
> }
> ```



---

_Comment by @epage on 2021-07-09 18:33_

I'm going to do an exercise in re-deriving (no pun intended) how all of this works, ignoring implementation.

The default case for struct fields is that they are arguments:
```rust
#[derive(Clap)]
struct Parent {
  field: Arg
}
```

`flatten` collapses structs
```rust
#[derive(Clap)]
struct Parent {
  field: Arg,
  #[clap(flatten)]
  delegate: Child,
}

#[derive(Clap)]
struct Child {
  parent_field: Arg
}
```

The default case for variants is that they are sub-commands and any contained value are arguments for that subcommand:
```rust
#[derive(Clap)]
enum Subcommands {
  First {
    field: Arg
  },
}
```

What gets interesting is the tuple case:
```rust
#[derive(Clap)]
enum Subcommands {
  First(Child)
}

#[derive(Clap)]
struct Child {
  field: Arg
}
```
Strictly speaking, `First(Child)` is a single-element tuple that is similar to `{child: Child}`,mainly different in how they are accessed (`.0` vs `.child`).  `structopt`/`clap` have been implicitly flattening `Child` into that tuple, making it feel like the difference is between anonymous and named struct, and giving an ergonomics boost (which is safe because tuples have no meaning in clap).

I know `structopt` docs focus mostly on the inline-struct syntax but I suspect that is for documentation brevity sake and most people use the implicit flattening (at least I never use that enum syntax, finding it cumbersome to access fields).   **So if I flip this around and talk about the implicit flattening case as the default, and the inline case as a shortcut, I propose the user model for subcommands is that variants are single-element tuple containing a child parser**.

From that perspective, an `enum` of subcommands or a `struct` of arguments are interchangeable for putting in the single-element tuple without any attributes. They are both parsers for whatever comes after the parent subcommand.  I would then say that `#[flatten]` on a variant would expect that variant to be more subcommands for the parent to delegate to.  

So I'd change the previous recommended behavior to:
> ```rust
> enum Foo {
>     // no attr, arguments to `foo`
>     Foo(ChildArgs),
>
>    // Shortcut syntax
>     FooAlt {
>       #[clap(subcommand)]
>       sub: Config
>     }
>
>     // no attr, subcommands to `bar`
>     Bar(ChildCommands),
>
>     // Be explicit, subcommands to `bar-alt`
>    #[clap(subcommand)]
>     BarAlt(ChildCommands),
>    
>     // Collapse `DelegatedCommands` into `Foo`
>    #[clap(flatten)]
>    Baz(DelegatedCommands),
> }
> ```



---

_Comment by @pksunkara on 2021-07-10 09:06_

I agree. I do want to point out that we need to allow explicit `subcommand` because it is needed if the root item is a struct instead of an enum.

---

_Comment by @pksunkara on 2021-07-10 09:10_

In fact, all of these cases work except for a non flattened enum inside enum which is what this issue is.

---

_Comment by @epage on 2021-07-10 23:25_

Yes, my intent was to to highlight the proposed principle for how all of these should behave in contrast to the other proposal.

---

_Referenced in [clap-rs/clap#2579](../../clap-rs/clap/pulls/2579.md) on 2021-07-12 20:38_

---

_Closed by @pksunkara on 2021-07-12 20:43_

---

_Reopened by @pksunkara on 2021-07-12 20:43_

---

_Comment by @pksunkara on 2021-07-12 20:43_

That PR auto-linked this issue since you had the word `fix` before the issue number in that PR's description.

---

_Comment by @epage on 2021-07-13 15:42_

For completeness sake, I wanted to compare what compiles for every case for `structop` as a baseline:
```rust
#!/usr/bin/env -S cargo eval --
//! ```cargo
//! [package]
//! edition = "2018"
//! [dependencies]
//! structopt = "*"
//! ```

#[derive(structopt::StructOpt)]
struct StructOfArgs {
  // Compile error: the trait bound `ChildArgs: FromStr` is not satisfied
  //implicit_args: ChildArgs,

  #[structopt(flatten)]
  flatten_args: ChildArgs,

  #[structopt(subcommand)]
  subcommand_args: ChildArgs,
}

#[derive(structopt::StructOpt)]
struct StructOfENums {
  // Compile error: the trait bound `Subcommand: FromStr` is not satisfied
  //implicit_subcmds: Subcommand,

  // Compile error: does not impl `ArgGroup`
  #[structopt(flatten)]
  flatten_subcmds: Subcommand,

  #[structopt(subcommand)]
  subcmmand_subcmds: Subcommand,
}

#[derive(structopt::StructOpt)]
enum EnumOfStructs {
  ImplicitArgs(ChildArgs),

  #[structopt(flatten)]
  FlattenArgs(ChildArgs),

  // Compile error: subcommand is only allowed on fields
  //#[structopt(subcommand)]
  //SubcommandArgs(ChildArgs),
}

#[derive(structopt::StructOpt)]
enum EnumOfEnums {
  ImplicitSubcmds(Subcommand),

  #[structopt(flatten)]
  FlattenSubcmds(Subcommand),

  // Compile error: subcommand is only allowed on fields
  //#[structopt(subcommand)]
  //SubcmmandSubcmds(Subcommand),
}

#[derive(structopt::StructOpt)]
struct ChildArgs {
}

#[derive(structopt::StructOpt)]
enum Subcommand {
}

fn main() {
  println!("Hello world");
}
```

---

_Comment by @epage on 2021-07-13 15:47_

And clap today (note: this doesn't capture what cases panic)
```rust
#!/usr/bin/env -S cargo eval --
//! ```cargo
//! [package]
//! edition = "2018"
//! [dependencies]
//! clap = "3.0.0-beta.2"
//! ```

#[derive(clap::Clap)]
struct StructOfArgs {
  // Compile error: the trait bound `ChildArgs: FromStr` is not satisfied
  //implicit_args: ChildArgs,

  #[clap(flatten)]
  flatten_args: ChildArgs,

  /* Compile error
error[E0277]: the trait bound `ChildArgs: clap::Subcommand` is not satisfied
 --> structopt.rs:8:10
  |
8 | #[derive(clap::Clap)]
  |          ^^^^^^^^^^ the trait `clap::Subcommand` is not implemented for `ChildArgs`
  |
  = note: required by `augment_subcommands`
  = note: this error originates in a derive macro (in Nightly builds, run with -Z macro-backtrace for more info)

error[E0277]: the trait bound `ChildArgs: clap::Subcommand` is not satisfied
  --> structopt.rs:16:10
   |
16 |   #[clap(subcommand)]
   |          ^^^^^^^^^^ the trait `clap::Subcommand` is not implemented for `ChildArgs`
   |
   = note: required by `from_subcommand`
  */
  //#[clap(subcommand)]
  //subcommand_args: ChildArgs,
}

#[derive(clap::Clap)]
struct StructOfENums {
  // Compile error: the trait bound `Subcommand: FromStr` is not satisfied
  //implicit_subcmds: Subcommand,

  #[clap(flatten)]
  flatten_subcmds: Subcommand,

  #[clap(subcommand)]
  subcmmand_subcmds: Subcommand,
}

#[derive(clap::Clap)]
enum EnumOfStructs {
  ImplicitArgs(ChildArgs),

  /* Compile errors:
error[E0277]: the trait bound `ChildArgs: clap::Subcommand` is not satisfied
  --> structopt.rs:33:10
   |
33 | #[derive(clap::Clap)]
   |          ^^^^^^^^^^ the trait `clap::Subcommand` is not implemented for `ChildArgs`
   |
   = note: required by `augment_subcommands`
   = note: this error originates in a derive macro (in Nightly builds, run with -Z macro-backtrace for more info)

error[E0277]: the trait bound `ChildArgs: clap::Subcommand` is not satisfied
  --> structopt.rs:33:10
   |
33 | #[derive(clap::Clap)]
   |          ^^^^^^^^^^ the trait `clap::Subcommand` is not implemented for `ChildArgs`
   |
   = note: required by `from_subcommand`
   = note: this error originates in a derive macro (in Nightly builds, run with -Z macro-backtrace for more info)
  */
  //#[clap(flatten)]
  //FlattenArgs(ChildArgs),

  // Compile error: subcommand is only allowed on fields
  //#[clap(subcommand)]
  //SubcommandArgs(ChildArgs),
}

#[derive(clap::Clap)]
enum EnumOfEnums {
  ImplicitSubcmds(Subcommand),

  #[clap(flatten)]
  FlattenSubcmds(Subcommand),

  // Compile error: subcommand is only allowed on fields
  //#[clap(subcommand)]
  //SubcmmandSubcmds(Subcommand),
}

#[derive(clap::Clap)]
struct ChildArgs {
}

#[derive(clap::Clap)]
enum Subcommand {
}

fn main() {
  println!("Hello world");
}
```

---

_Comment by @epage on 2021-07-13 16:30_

This is mostly me thinking aloud to help save the information and in case anyone else has thoughts.

`structopt` had `flatten` and `subcommand` attributes which are nearly interchangeable.  To support distinguishing `Option<SubCmd>` from `SubCmd`, the parent structure did a runtime check `is_subcommand` to know whether to set `SubcommandRequiredElseHelp`.

I think the original intent in #1681 was to make these distinct by having a separate `Subcommand` trait.  `flatten` would access the `IntoApp` trait which a `Subcommand` wouldn't have and would give a decent compiler error.  We mostly hide these traits though, so people instead just derive `Clap` which also gives `IntoApp`, negating the error reporting.

This leads us to a situation where there are no compile time errors and, without `subcommand` attribute which sets up validation, if the subcommand isn't specified, we `unwrap` and crash.

Possible solutions:

## Go back to the original behavior

This requires
- A way for the caller to control optional/required for subcommands
- `from` to handle optional values

With this option, `Subcommand` isn't really buying us anything anymore and would be removed.

Instead of the original solution for this, my thought is we'd change the traits so:
```
    // Old: fn augment_clap(app: App<'_>) -> App<'_>;
    fn augment_clap(app: App<'_>, required: bool) -> App<'_>;
    // Old: fn from_arg_matches(matches: &ArgMatches) -> Self;
    fn from_arg_matches(matches: &ArgMatches) -> Optional<Self>;
```
The advantage of this route is the we isolate all arg group / subcommand knowledge to the trait implementer rather than the caller having to know some.  The disadvantage is this doesn't scale with other features but we don't have a clear enough idea of what those are, so YAGNI

We could go all in on this and merge `flatten` / `subcommand` and drop having the subtle-to-non-existent difference.  The challenge would be in the name.  `delegate`?

## Go all in on #1681

I feel #1681 only went part way.  It created a `Subcommand` trait but we also need to move the flatten logic out of `IntoApp` and into a `Args` trait.

The division of labor would be
- `IntoApp` for top-level structs/enums
- `Subcommand` for use with `#[clap(subcommand)]`
- `Args` for structs to use with `#[clap(flatten)]` and to be the implicit behavior with 1-tuple's in enums
  - Maybe we'd automatically create an argument group out of this, where the group name is the struct name
  - This would allow us to `#[derive(Args)]` on enums for the case of "only accept one of these arguments"
  - I wpuld have named this `ArgGroup` except a struct exists with that name
  - Another issue is that people would now want to directly derive `Subcommand` vs `Args` since an enum could use one or the other.  `Subcommand` is self-contained (it has `from`).  If we make them not self-contained, then users would have to derive two traits.  We could either duplicate the functions between the traits or make `Subcommand` / `Args` super traits that implicitly derive `FromArgMatches`, like deriving `Clap` does.

In this case, I would `clap(flatten)` behave differently, based on whether it is used in a `Subcommand` or `Args`, rather than add a `subcommand(flatten)`.

## Other notes

- It is incongruent that we have separate `IntoApp` / `FromArgsMatches` and yet `Subcommand` mixes the two
- Similarly, seems odd for the struct `augment` functions live on `IntoApp`
- As someone who maintains clap argument crates, I'd like to derive the lower level traits and not allow mine to be used as a top-level trait.
- At least for `augment` functions, it seems we generate a lot of duplicate code and bloat is a common concern with `clap`.  What if we just had an `update` argument?

And a general observation is that merging `structopt` into `clap` is exposing a lot of macro implementation details that, to change, will require breaking compatibility, more so than the current `App` API for `clap` requires.

## Proposal

I'm changing my thought on my last proposal to
```rust
enum Foo {
    // no attr, arguments to `foo`
    Foo(ChildArgs),

    // Shortcut syntax
    FooAlt {
      #[clap(subcommand)]
      sub: Config
    }

    // Be explicit, subcommands to `bar-alt`
    #[clap(subcommand)]
    BarAlt(ChildCommands),

    // Collapse `DelegatedCommands` into `Foo`
    #[clap(flatten)]
    Baz(DelegatedCommands),
}
```

The matrix will then be:

| Case                      | structopt      | clap-3.0.0-beta2  | proposed          |
|---------------------------|----------------|-------------------|-------------------|
| struct/struct, no-attr    | Error: FromStr | Error: FromStr    | Error: FromStr    |
| struct/struct, flatten    | \-             | \-                | \-                |
| struct/struct, subcommand | \-             | Error: Subcommand | Error: Subcommand |
| struct/enum, no-attr      | Error: FromStr | Error: FromStr    | Error: FromStr    |
| struct/enum, flatten      | \-             | \-                | Error: Args       |
| struct/enum, subcommand   | \-             | \-                | \-                |
| enum/struct, no-attr      | \-             | \-                | \-                |
| enum/struct, flatten      | \-             | Error: Subcommand | Error: Subcommand |
| enum/struct, subcommand   | Error: fields  | Error: fields     | Error: Subcommand |
| enum/enum, no-attr        | \-             | Panic             | \-                |
| enum/enum, flatten        | \-             | \-                | \-                |
| enum/enum, subcommand     | Error: fields  | Error: fields     | \-                |

This will be done with
```rust
trait IntoApp: Sized {
    fn into_app<'help>(update: bool) -> App<'help>;
}

trait FromArgMatches: Sized {
    fn from_arg_matches(matches: &ArgMatches) -> Option<Self>;

    fn update_from_arg_matches(&mut self, matches: &ArgMatches);
}

// Also derives FromArgMatches
trait Args: FromArgMatches + Sized {
    fn append_args(app: App<'_>, update: bool) -> App<'_>;
}

// Also derives FromArgMatches
trait Subcommand: FromArgMatches + Sized {
    fn append_subcmmand(app: App<'_>, update: bool) -> App<'_>;
}
```

- `flatten` and `subcommand` wouldn't be interchangeable in structs
- Provide errors about missing trait impls when wrong attribute is used
- Easy to derive relevant subsets for crates like `clap-cargo`, `clap-verbosity`, etc
- for `derive(Args)`, in the future we could add container attributes for specifying the struct is a group and to override the group's name
- Opens the door for `#[derive(Args)] enum {}` for mutually exclusive arguments (implicitly a group)
- Merging with `update` variants is deferrable, maybe rejectable.

---

_Comment by @epage on 2021-07-13 17:40_

In having 1-3 top-level traits, I think it could make sense to give `clap::Clap` a more meaningful name to help clarify the roles.  My thought would be `clap::Parser`.

---

_Referenced in [clap-rs/clap#2583](../../clap-rs/clap/pulls/2583.md) on 2021-07-13 18:14_

---

_Comment by @pksunkara on 2021-07-13 23:20_

I would prefer to go all in on #1681. There were quite a few reasons in previous issues regarding this which happened before I even learnt rust.

I completely agree with your proposed matrix.

> Easy to derive relevant subsets for crates like clap-cargo, clap-verbosity, etc

Can you please expand on that sentence?

---

That said, I strongly disagree with asking the users to derive either `Args` or `Subcommand` or `Parser` or a combination of them. We should aim to simplify the user experience as much as possible by asking them to simply derive one (`Clap`) which magically implements the underlying the traits depending on what kind of item it gets.

Of course, the user always have the flexibility of deriving only those traits.

---

_Comment by @epage on 2021-07-14 00:53_

> Easy to derive relevant subsets for crates like clap-cargo, clap-verbosity, etc

[`clap_cargp::Features`](https://github.com/crate-ci/clap-cargo/blob/master/src/features.rs#L3) derives `Structopt`, which means it can both be used as a top-level parser (`clap_cargo::Features::parse` via`structopt::Structop::parse`) and when flattening into another parser.  This doesn't make sense and it would be nice if we could make the intent clearer that this is only intended for use in flattening into a parser.

> Of course, the user always have the flexibility of deriving only those traits.

They can't derive them all currently.  Today, a user would  `derive(clap::Subcommand)` for an eum or `derive(clap::IntoApp)` (and **manually** implement `clap::FromArgMatches`) for a struct.

> That said, I strongly disagree with asking the users to derive either Args or Subcommand or Parser or a combination of them. 
> ...
> We should aim to simplify the user experience as much as possible by asking them to simply derive one (Clap) which magically implements the underlying the traits depending on what kind of item it gets.

My proposal is that `clap::Clap` (renamed `clap::Parser`)  can still be used in the same way
- For enums, it assumes `Subcommand` and derives it, `IntoApp`, and `FromArgMatches`
- For structs, it assumes `Args` and derives it, `IntoApp`, and `FromArgMatches`

The user can then **optionally** choose to derive `Subcommand` or `Args` instead (which will automatically derive `FromArgMatches`).

Most of this falls out of the work I'm doing for this issue.

`ArgEnum` is the one that still stands out to me.  I'm assuming this is "magic" that you can `derive(Clap)` on an enum and it will implement `ArgEnum` and allow you to use an enum as an argument.   If so, this does not simplify things for the user but makes it more complicated.

Every time I've interacted with a crate with a derive that provides functionality beyond deriving the trait that I named, I've been confused and feel uncertainty on how to use it.  `derive`s are opaque to the end-user and they can't tell what is being generated, without extra work which is non-obvious work, especially to newer Rust users (a prime persona for clap).  When they know its just a trait, they can look it up and be done.  When its unrelated traits or functions, there is no way to predict this, to lookup that API, etc.

In addition, this is giving your types that don't make sense.  There is never a time a user would be wanting an `enum` to act as both an argument value and a subcommand set.

Personally, I can go either way on `clap::Clap` deriving `Subcommand` and `Args`.  It is a big of "magic" but its easily understood I believe because it has more of a is-a relationship, even if its not a super-trait.

One case for why emphasizing `Args` and `Subcommand` are important is it provides a path for the user to understand and use these or other traits in ways `derive(Clap)` won't provide.  The example I gave in an earlier post is `#[derive(Args)] enum MutuallyExclusiveArgGroup {}` (described as ["the most common use" for `ArgGroup`](https://docs.rs/clap/2.33.3/clap/struct.ArgGroup.html#examples)).  This is a natural next step from my work on this Issue.  `derive(Clap)` cannot cover this case (unless we add magic attributes to change what traits it derives).  Under the current scheme, `derive(Clap)` is the only concept introduced to the user and they have no concept of other traits they can impl to further customize the behavior.  If we instead are upfront about `Subcommand` and `Args`, it becomes natural for them to consider using these in other ways, look at their docs, or look for related traits they can derive.

Besides making other traits top-level, part of my suggestion for renaming `Clap` to `Parser` is to make the intent clear.  Seeing the name `Clap` doesn't tell me what it does or how I'd user it.  Seeing the name `Parser` does.

---

_Comment by @pksunkara on 2021-07-14 09:38_

> This doesn't make sense and it would be nice if we could make the intent clearer that this is only intended for use in flattening into a parser.

👍 

> My proposal is that clap::Clap (renamed clap::Parser) can still be used in the same way

That sounds good to me. I must have misunderstood your original proposal. I still don't see a strong reason on the rename and would prefer `Clap` (but we can revisit this discuss later).

> `ArgEnum` is the one that still stands out to me

I am okay with `Clap` not deriving `ArgEnum` automatically.

> Besides making other traits top-level

I think this is already the way we do things. If you see `lib.rs`, the `Subcommand` and `IntoApp` can be derived independently.

> Most of this falls out of the work I'm doing for this issue.

I would say implementing your proposal for the traits would be part of the issue fix here.

---

I think both of us are aligned (except for the rename). Please feel free to go ahead with the implementation. I would prefer if you can send small PRs but I don't mind if that can't be done.

---

_Referenced in [clap-rs/clap#2584](../../clap-rs/clap/issues/2584.md) on 2021-07-14 15:29_

---

_Referenced in [clap-rs/clap#2586](../../clap-rs/clap/pulls/2586.md) on 2021-07-14 17:36_

---

_Referenced in [clap-rs/clap#2587](../../clap-rs/clap/pulls/2587.md) on 2021-07-14 17:43_

---

_Closed by @pksunkara on 2021-07-16 20:39_

---

_Referenced in [informalsystems/modelator#59](../../informalsystems/modelator/pulls/59.md) on 2021-08-16 10:53_

---

_Referenced in [clap-rs/clap#2814](../../clap-rs/clap/pulls/2814.md) on 2021-10-05 14:49_

---
