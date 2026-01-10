---
number: 2255
title: "AppSettings::SubcommandsNegateReqs may cause panic in Clap::parse(), Clap::try_parse()"
type: issue
state: closed
author: BeldrothTheGold
labels:
  - A-docs
assignees: []
created_at: 2020-12-14T02:16:28Z
updated_at: 2021-10-26T20:56:17Z
url: https://github.com/clap-rs/clap/issues/2255
synced_at: 2026-01-10T01:27:14Z
---

# AppSettings::SubcommandsNegateReqs may cause panic in Clap::parse(), Clap::try_parse()

---

_Issue opened by @BeldrothTheGold on 2020-12-14 02:16_

### Clap version

3.0.0-beta.2

### Where

https://docs.rs/clap/3.0.0-beta.2/clap/enum.AppSettings.html#variant.SubcommandsNegateReqs

### What's wrong

There's no mention that this AppSetting can cause the Clap derive macro to panic on parse() and try_parse()

### How to fix

```
use clap::{AppSettings, Clap, IntoApp, FromArgMatches};

#[derive(Debug, Clap)]
#[clap(setting(AppSettings::SubcommandsNegateReqs))]
struct ExOpts {
    
    req_str: String,

    #[clap(subcommand)]
    pub cmd: Option<SubCommands>,
}

#[derive(Debug, Clap)]
enum SubCommands{
    ExSub {
        #[clap(short, long, parse(from_occurrences))]
        verbose: u8
    },
}

fn main(){

    // We cant use Clap::parse or try_parse because we have SubcommandsNegateReqs enabled
    // this would cause a panic when Clap attempts to parse the req_str arg that isn't there
    // let opts = ExOpts::parse(); // panics
    // let opts = ExOpts::try_parse(); // panics 

    // Instead we need to check to see if a subcommand exists before we run from_arg_matches on ExOpts
    let matches = ExOpts::into_app().get_matches();
    if matches.subcommand_name().is_none() {
        // If no subcommand we can derive ExOpts
        let opts = ExOpts::from_arg_matches(&matches);
        println!("{:?}", opts);
    } else {
        // If subcommand we need derive the subcommands instead
        let cmd_opts = SubCommands::from_arg_matches(&matches);
        println!("{:?}", cmd_opts);
    }

}
```


---

_Label `C: docs` added by @BeldrothTheGold on 2020-12-14 02:16_

---

_Added to milestone `3.0` by @pksunkara on 2020-12-14 02:17_

---

_Comment by @pksunkara on 2020-12-14 02:17_

What's the panic message you were getting?

---

_Comment by @BeldrothTheGold on 2020-12-14 02:18_

thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', src/main.rs:7:14
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---

_Comment by @pksunkara on 2020-12-14 02:19_

Try changing to the following and describe what happens (with error message):

```rust
    #[clap(required = true)]
    req_str: Option<String>,
```

---

_Comment by @pksunkara on 2020-12-14 02:21_

Definitely needs to be documented though.

---

_Comment by @BeldrothTheGold on 2020-12-14 02:24_

Oh wait. Haha, sorry. No it still panics. Sorry. 

$ cargo run -- ex-sub
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/clap-ex ex-sub`
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', src/main.rs:9:14
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

```
#[allow(unused_imports)]
use clap::{AppSettings, Clap, IntoApp, FromArgMatches};

#[derive(Debug, Clap)]
#[clap(setting(AppSettings::SubcommandsNegateReqs))]
struct ExOpts {

    #[clap(required = true)]
    req_str: String,

    #[clap(subcommand)]
    pub cmd: Option<SubCommands>,
}

#[derive(Debug, Clap)]
enum SubCommands{
    ExSub {
        #[clap(short, long, parse(from_occurrences))]
        verbose: u8
    },
}

fn main(){

    // We cant use Clap::parse or try_parse because we have SubcommandsNegateReqs enabled
    // this would cause a panic when Clap attempts to parse the req_str arg that isn't there
    let opts = ExOpts::parse(); // panics
    println!("{:?}", opts);

}
```

---

_Comment by @pksunkara on 2020-12-14 02:26_

It's probably better to find a way to get this working though.

---

_Comment by @pksunkara on 2020-12-14 02:26_

Huh, you haven't done `req_str: Option<String>`. Please try with that.

---

_Comment by @BeldrothTheGold on 2020-12-14 02:29_

This does not compile. 

```rust
    #[clap(required = true)]
    req_str: Option<String>,
```
```
error: required is meaningless for Option
 --> src/main.rs:8:12
  |
8 |     #[clap(required = true)]
  |            ^^^^^^^^

error: aborting due to previous error
```
this does compile but req_str is no longer required
```rust
    req_str: Option<String>
```
```
 cargo run -- 
   Compiling clap-ex v0.1.0 (/home/scummin2/dev/rust/clap-ex)
    Finished dev [unoptimized + debuginfo] target(s) in 0.52s
     Running `target/debug/clap-ex`
Ok(ExOpts { req_str: None, cmd: None })
```

---

_Comment by @BeldrothTheGold on 2020-12-14 02:32_

At the very least it might be a good idea to make a try_from_arg_matches() fn which can bubble up any issues so try_parse doesn't panic 

---

_Renamed from "Document AppSettings::SubcommandsNegateReqs may cause panic in Clap::parse(), Clap::try_parse()" to "AppSettings::SubcommandsNegateReqs may cause panic in Clap::parse(), Clap::try_parse()" by @BeldrothTheGold on 2021-02-15 03:04_

---

_Comment by @BeldrothTheGold on 2021-02-15 03:05_

found this related issue in structopt repo https://github.com/TeXitoi/structopt/issues/124 

---

_Referenced in [clap-rs/clap#2614](../../clap-rs/clap/pulls/2614.md) on 2021-07-21 21:54_

---

_Comment by @epage on 2021-10-05 15:01_

tl;dr
- We can't make the listed example work because we don't have a way to instantiate `req_string`
- If we update our derive to allow `required = true` with `Option`, this gives people the flexibility to create their own solution for this
- As this is development time, I think a panic is acceptable (its like an assert)
  - Unrelated to this, we should also be allowing proper error reporting, rather than panicing, for our derives

Did a quick update to this to clarify things for me
```rust
use clap::AppSettings;
use clap::Clap;
use clap::FromArgMatches;
use clap::IntoApp;

#[derive(Debug, clap::Clap)]
#[clap(setting(AppSettings::SubcommandsNegateReqs))]
struct ExOpts {
    req_str: String,

    #[clap(subcommand)]
    pub cmd: Option<SubCommands>,
}

#[derive(Debug, clap::Subcommand)]
enum SubCommands {
    ExSub {
        #[clap(short, long, parse(from_occurrences))]
        verbose: u8,
    },
}

fn main() {
    // Reproduction cases
    // - `cargo run -- ex-sub`
    if true {
        // We cant use Clap::parse or try_parse because we have SubcommandsNegateReqs enabled
        // this would cause a panic when Clap attempts to parse the req_str arg that isn't there
        let opts = ExOpts::parse(); // panics
        let opts = ExOpts::try_parse(); // panics
    } else {
        // Instead we need to check to see if a subcommand exists before we run from_arg_matches on ExOpts
        let matches = ExOpts::into_app().get_matches();
        if matches.subcommand_name().is_none() {
            // If no subcommand we can derive ExOpts
            let opts = ExOpts::from_arg_matches(&matches);
            println!("{:?}", opts);
        } else {
            // If subcommand we need derive the subcommands instead
            let cmd_opts = SubCommands::from_arg_matches(&matches);
            println!("{:?}", cmd_opts);
        }
    }
}
```

The big problem is we have no way to instantiate `ExOpts`.  I view this as a development-time error and assert-like behavior is acceptable (a panic).

If we switch to
```
#[derive(Debug, clap::Clap)]
#[clap(setting(AppSettings::SubcommandsNegateReqs))]
struct ExOpts {
    req_str: Option<String>,

    #[clap(subcommand)]
    pub cmd: Option<SubCommands>,
}
```
There is no longer a panic but a field is no longer required!

Clap will error though if you do
```rust
#[derive(Debug, clap::Clap)]
#[clap(setting(AppSettings::SubcommandsNegateReqs))]
struct ExOpts {
    #[clap(required = true)]
    req_str: Option<String>,

    #[clap(subcommand)]
    pub cmd: Option<SubCommands>,
}
```
with
```
error: required is meaningless for Option
 --> src/main.rs:9:12
  |
9 |     #[clap(required = true)]
  |            ^^^^^^^^
```
Apparently, its not meaningless.

I do however feel that we should have a non-panicing way of doing our derive but for a different use case.  I maintain `clap-cargo`, a reusable set of command line flags to imitate cargo.  I want people to be able to use the builder API with it which means we have less control over the behavior and should be more careful about panicing.

---

_Comment by @kbknapp on 2021-10-05 17:01_

IMO the correct way forward here is to allow `#[clap(required = true)] req_string: Option<String>`, so we remove our compile error. *Technically* the `String` in the example should be an `Option` since if a subcommand is used, it has no value (other possibilities would be having a default value, or impl `Default`). 

The hard hard becomes how do we detect that and not panic? Or rather, I'm OK with `panic`ing, so long as the message is clear as to why, and how to fix it. The easiest method is probably to have a validation step that gets run during or at the end of `derive` which looks for any problematic cases and can error (or panic since its a developer compile time error). 

For now, that validation phase would only include a single check of is `AppSettings::SubcomamndsNegateReqs` used with a non-`Option` field (or non-`Default`/non-`default_value`). That's probably a little harder to implement than I'm making it sound, but I think that'd be a good way forward.

---

_Referenced in [clap-rs/clap#2817](../../clap-rs/clap/pulls/2817.md) on 2021-10-05 19:22_

---

_Comment by @epage on 2021-10-05 20:24_

Just recording some thoughts in looking at the code.

At the moment, the key signature here is
```rust
    fn from_arg_matches(matches: &ArgMatches) -> Option<Self>;
```
The `Option` is currently used to test which `#[flatten] subcmd` matches the current command.  Reusing it for other logic would muddy the waters.  However, we could refactor this to instead use `has_subcommand` and we'd no longer attach meaning to the `Option` and it can be either an `Option` or `Result` and we can start passing up the stack.

However, unless we start attaching stack traces to our errors, this will make debugging a development time issue pretty difficult (no indication of what is causing the failure).  Maybe we should attach that as part of the `debug` feature or something.

---

_Referenced in [clap-rs/clap#2820](../../clap-rs/clap/pulls/2820.md) on 2021-10-05 20:37_

---

_Comment by @epage on 2021-10-05 23:59_

Pulling this over from #2820

> Granted, as we discussed in that issue, the developer _should_ use an `Option` + `clap(required = true)`, but that is not exactly intuitive. And the `panic` message that is emitted doesn't point them in the right direction either (using the test case in the linked issue):
> 
> ```
> thread 'required_option_type' panicked at 'app should verify arg is required', clap_derive/tests/options.rs:346:18
> ```
> 
> We should probably change that to something generic-ish like `arg 'X' should be of type Option<T>, and manually add #[clap(required = true)] due to conflicting attributes`. This doesn't say what those conflicting attributes are, because I'm not sure we want to try and enumerate all cases or detect which one triggered it, but at least points them in the right direction.

For the message "app should verify arg is required", all we know is something didn't work right and we expected a `Some(_)` but got a `None`.  This could be because
- A bug in `clap_derive`
- The user uncovered an unexpected combination of flags (like this Issue)
- The user is calling `from_arg_matches` directly on the struct but set flags that clap_derive` didn't expect
  - This might sound strange but something I'm wanting to eventually see is having crates like `clap-cargo` be usable by both the derive and non-derive API (which is why I want to switch us to error handling over panic)

We could a combination of
- Give preference to one error case to smooth it out, confusing users in other, more rare, error cases
- Pattern match against known failures and provide suggestions for the user (only reactive, still could lead to other confusing cases)
- Switch to error reporting and have `debug` feature flag also add backtraces to our errors (this just moves the problem but doesn't help with the core of it)
- Regardless of panic or error, we can restructure the code to include more information in the panic (what field on what struct).  This will help bloat binaries though (at least it doesn't show up in our crate ;) ).

---

_Comment by @kbknapp on 2021-10-06 00:09_

> For the message "app should verify arg is required", all we know is something didn't work right and we expected a Some(_) but got a None. 

This message is emitted if the developer *doesn't* use an `Option`, i.e. 

```rust
#[derive(Debug, Clap)]
#[clap(setting(AppSettings::SubcommandsNegateReqs))]
struct ExOpts {

    #[clap(required = true)]
    req_str: String,

    #[clap(subcommand)]
    pub cmd: Option<SubCommands>,
}

#[derive(Debug, Clap)]
enum SubCommands{
    ExSub {
        #[clap(short, long, parse(from_occurrences))]
        verbose: u8
    },
}
```
Which is why I think the "you should verify arg is required" leaves one like....wut? :smile: 

---

_Comment by @epage on 2021-10-06 00:12_

> Which is why I think the "you should verify arg is required" leaves one like....wut? smile

I know its easy to miss but its "app", not "you", as in we expect we created the `App`s validation to prevent this case.

---

_Referenced in [clap-rs/clap#2827](../../clap-rs/clap/pulls/2827.md) on 2021-10-06 19:34_

---

_Referenced in [clap-rs/clap#2888](../../clap-rs/clap/pulls/2888.md) on 2021-10-15 20:45_

---

_Referenced in [clap-rs/clap#2890](../../clap-rs/clap/pulls/2890.md) on 2021-10-16 00:40_

---

_Assigned to @epage by @epage on 2021-10-25 21:02_

---

_Referenced in [clap-rs/clap#2943](../../clap-rs/clap/pulls/2943.md) on 2021-10-25 23:42_

---

_Closed by @bors[bot] on 2021-10-26 20:56_

---

_Referenced in [clap-rs/clap#3118](../../clap-rs/clap/issues/3118.md) on 2021-12-09 16:20_

---
