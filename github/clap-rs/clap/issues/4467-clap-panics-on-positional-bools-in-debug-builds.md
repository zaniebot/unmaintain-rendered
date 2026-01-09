---
number: 4467
title: Clap panics on positional bools in debug builds
type: issue
state: closed
author: therealprof
labels:
  - C-bug
  - M-breaking-change
  - A-derive
  - S-wont-fix
assignees: []
created_at: 2022-11-08T12:26:21Z
updated_at: 2025-08-15T13:42:34Z
url: https://github.com/clap-rs/clap/issues/4467
synced_at: 2026-01-07T13:12:20-06:00
---

# Clap panics on positional bools in debug builds

---

_Issue opened by @therealprof on 2022-11-08 12:26_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.64.0 (a55dd71d5 2022-09-19)

### Clap Version

4.0.22

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Parser)]
#[clap(author, version, name = "test")]
struct Test {
    #[clap(subcommand)]
    action: Action,
}

#[derive(Subcommand)]
enum Action {
    #[clap(name = "encode_msg")]
    EncodeMsg {
        msgid: String,
        #[clap(subcommand)]
        typ: MsgType,
    },
}

#[derive(Parser)]
enum MsgType {
    #[clap(name = "Notify")]
    USPNotify {
        sub_id: String,
        send_resp: bool,
        #[clap(subcommand)]
        typ: NotifyType,
    },
}

#[derive(Parser)]
pub enum NotifyType {
    Request { oui: String },
}

fn main() -> () {
    Test::parse();
}
```

### Steps to reproduce the bug with the above code

```
# cargo run --release encode_msg Foo Notify foo false request foo
```

### Actual Behaviour

```
error: The argument '[SEND_RESP]' requires 0 values, but 1 was provided

Usage: test encode_msg <MSGID> Notify <SUB_ID> [SEND_RESP] <COMMAND>

For more information try '--help'
```

### Expected Behaviour

`send_resp` shouldn't be rendered as an optional argument in the synopsis and if supplied should result in the parsed variable to be false.

### Additional Context

As a bonus bug:

If run like this, the synopsis changes:
```
# cargo run --release encode_msg Foo Notify foo --send_resp Bar
    Finished release [optimized] target(s) in 0.04s
     Running `target/release/test encode_msg Foo Notify foo --send_resp Bar`
error: Found argument '--send_resp' which wasn't expected, or isn't valid in this context

  If you tried to supply '--send_resp' as a value rather than a flag, use '-- --send_resp'

Usage: test encode_msg <MSGID> Notify <SUB_ID|SEND_RESP> <COMMAND>
```

And in dev mode it panics:
```
# cargo run encode_msg Foo Notify foo false request foo
    Finished dev [unoptimized + debuginfo] target(s) in 0.05s
     Running `target/debug/test encode_msg Foo Notify foo false request foo`
thread 'main' panicked at 'Argument 'send_resp` is positional, it must take a value', /Users/egger/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.22/src/builder/debug_asserts.rs:740:9
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

### Debug Output

_No response_

---

_Label `C-bug` added by @therealprof on 2022-11-08 12:26_

---

_Comment by @epage on 2022-11-10 04:03_

`bool` is defined in the reference to [default to flag-like behavior](https://docs.rs/clap/latest/clap/_derive/index.html#arg-types) and you need to override what clap is doing implicitly.

That said, we can look into detecting if its positional (no shorts or longs) and skip that.  I have some hesitance around this as we might not have all of the information we need in all of the right places.

I also think we should have a debug_assert about a positional argument taking 0 values to help catch this earlier.

---

_Label `A-derive` added by @epage on 2022-11-10 04:04_

---

_Label `S-waiting-on-decision` added by @epage on 2022-11-10 04:04_

---

_Referenced in [clap-rs/clap#4471](../../clap-rs/clap/pulls/4471.md) on 2022-11-10 04:11_

---

_Comment by @epage on 2022-11-10 04:12_

It looks like we do have an existing assert for this.  For me, in debug builds, I get a panic.

We generally recommend having a test that does
```rust
use clap::CommandFactory;
Test::command().debug_assert()
```
to help catch these

---

_Comment by @therealprof on 2022-11-10 12:42_

> It looks like we do have an existing assert for this. For me, in debug builds, I get a panic.

Yes, this is mentioned in the bug report above.

I used your `debug_assert()` but it doesn't seem to change anything in the output.

```
# cargo run encode_msg Foo Notify foo --send_resp Bar
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `target/debug/test encode_msg Foo Notify foo --send_resp Bar`
thread 'main' panicked at 'Argument 'send_resp` is positional, it must take a value', /Users/egger/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.22/src/builder/debug_asserts.rs:740:9
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Comment by @therealprof on 2022-11-10 12:54_

> `bool` is defined in the reference to [default to flag-like behavior](https://docs.rs/clap/latest/clap/_derive/index.html#arg-types) and you need to override what clap is doing implicitly.

Not sure I understand. I do want that describe behaviour, don't I? However it is not what actually seems to happen, when I execute the command as described **without** supplying a `send_resp` flag or boolean value, the actual value of the variable after parsing is always false. That's how I found the bug in the first place since I **do** want it to be `true` and failed to change it from the command line.

> I also think we should have a debug_assert about a positional argument taking 0 values to help catch this earlier.

I do think this assert is rather useless since we're talking about a command line parsing library here and the only people who'd even have a remote chance of catching this are the developers who're doing the `cargo run` dance which is a bit annoying since `cargo` interferes with command line parsing quite a bit; to test things I'm pretty much always using `cargo install --path .` instead to get the end-user POV, however that uses `--release` mode so I don't get the assert.

---

_Comment by @epage on 2022-11-10 13:09_

> Not sure I understand. I do want that describe behaviour, don't I? However it is not what actually seems to happen, when I execute the command as described without supplying a send_resp flag or boolean value, the actual value of the variable after parsing is always false. That's how I found the bug in the first place since I do want it to be true and failed to change it from the command line.

Are you wanting a flag or a positional value?

For a flag, you also need to opt-in to `#[arg(short)]` and/or `#[arg(long)]` ([tutorial](https://docs.rs/clap/latest/clap/_derive/_tutorial/index.html#flags))

For a positional, you need to override the default `ArgAction::SetTrue` (doesn't accept values) to `ArgAction::Set` (accepts values, picking parser based on the field type), like `#[arg(action = clap::ArgAction::Set)]`.

> I do think this assert is rather useless since we're talking about a command line parsing library here and the only people who'd even have a remote chance of catching this are the developers who're doing the cargo run dance which is a bit annoying since cargo interferes with command line parsing quite a bit; to test things I'm pretty much always using cargo install --path . instead to get the end-user POV, however that uses --release mode so I don't get the assert.

- In my comment, I called out that we recommend having a test explicitly activate these.  The tutorial includes [an example](https://docs.rs/clap/latest/clap/_derive/_tutorial/index.html#testing)
- Ideally, you'd have end-to-end tests.   I use a mixture of [snapbox](https://docs.rs/snapbox/latest/snapbox/) and [trycmd](https://docs.rs/trycmd) for mine
- I've never had problems with `cargo run`.   In fact, I have all of my own personal CLIs wrapped in shell scripts that are in PATH that invoke them via `cargo run` so that I am always testing HEAD.  Are you escaping your args with `--`, for example `cargo run --manifest-path ~/src/personal/cargo-release/Cargo.toml -- "$@"`

---

_Comment by @therealprof on 2022-11-10 13:27_

> Are you wanting a flag or a positional value?

I don't really care, I was hoping for a positional (the documentation in https://docs.rs/clap/latest/clap/_derive/index.html#arg-types does call the effect a flag though -- might want to straighten out terminology a bit) but the synopsis indicated that this was somehow optional. Then I played around with specifying it as flag, suddently changing the synopsis to something completely different.
(cf. above):
`Usage: test encode_msg <MSGID> Notify <SUB_ID> [SEND_RESP] <COMMAND>`
vs.
`Usage: test encode_msg <MSGID> Notify <SUB_ID|SEND_RESP> <COMMAND>`
 
I don't really understand why the default action is a non-working `.action(ArgAction::SetTrue)`, if it's not specified I would definitely expect to see the value coming out as `true`.

`ArgAction::Set` works a treat, but it's totally not obvious that this is what needs doing to get a positional boolean going. And it's also not obvious why the `bool` parameter is implicitly treated as optional; if I had wanted an optional positional parameter I'd have tried `Option<bool>`.

---

_Comment by @epage on 2022-11-10 14:09_

All of what you saw comes back to the general assumption that `bool` will be used as a flag and that the caller will set `short` and/or `long`.  As I mentioned earlier, we can look into making the default smarter by checking if its being used without `short` or `long` and instead using `Set`.  My only concern is piping the information through for that to happen which, looking back, might not be too bad.

---

_Comment by @therealprof on 2022-11-10 14:20_

I think at least the documentation could be clarified to point out that pitfall and maybe provide an example for positional bool.

---

_Comment by @DoumanAsh on 2023-05-08 10:59_

> bool is defined in the reference to default to flag-like behavior and you need to override what clap is doing implicitly.

It should not be the case, this generated broken parser

```
use clap::Parser;

#[derive(Parser, Debug)]
struct RetardedArgs {
    #[clap(value_parser)]
    enabled: bool,
}
fn main() {
    let result = RetardedArgs::parse();
    println!("result={:?}", result);
}
```

And you do not generate compile error, even though this parser never going to work
Why are you doing this?

---

_Comment by @epage on 2023-05-08 11:55_

> And you do not generate compile error, even though this parser never going to work
> Why are you doing this?

#3864 is a similar case where we could move the error to be at compile time.  #3133 represents the case where this is a lot more difficult.

Downsides to moving things to compile errors
- It won't cover everything in `Command::debug_assert` and people will get a false sense of security.  You still need to be creating the tests.  We are discussing creating the test automatically in #4838
- We are duplicating the logic and will need to duplicate the tests, increasing the maintenance burden and risk of not keeping them in sync with each other
- We would be slowing down compile times for people (time to build `clap_derive`, time to run `clap_derive`).  Unsure how noticeable this would be but our primary focus right now is on compile times.

---

_Referenced in [clap-rs/clap#4895](../../clap-rs/clap/pulls/4895.md) on 2023-05-08 12:15_

---

_Comment by @DoumanAsh on 2023-05-08 12:15_

Then do not force flag behavior for boolean, it is simple as that.

You do not slow compile times.
It is already slow because cargo cannot comprehend that it needs to build proc macros in release mode always
So you don't need to worry about that.

Please see example solution
https://github.com/clap-rs/clap/pull/4895

---

_Comment by @epage on 2023-05-08 12:26_

> Then do not force flag behavior for boolean, it is simple as that.

That is not what people will generally want with `bool` though.  Positional bools should also be relatively rare because the intent is unclear from their position.

---

_Comment by @DoumanAsh on 2023-05-08 12:29_

It depends on your use case.
But boolean default behavior should be dependent on context (whether it is flag or positional) as with any other type
You should not specialize boolean just because most people use it as flag.

Rejecting good UX just because you want to force your opinion about boolean on others is not good

---

_Referenced in [clap-rs/clap#5135](../../clap-rs/clap/issues/5135.md) on 2023-09-25 11:59_

---

_Renamed from "Clap seems easily confused by boolean arguments" to "Clap panics on positional bools in debug builds" by @epage on 2023-09-25 12:01_

---

_Label `S-waiting-on-decision` removed by @epage on 2025-08-15 13:39_

---

_Label `S-wont-fix` added by @epage on 2025-08-15 13:39_

---

_Label `M-breaking-change` added by @epage on 2025-08-15 13:39_

---

_Comment by @epage on 2025-08-15 13:42_

In reviewing this, the only solution would be to change the derive for `bool` to not imply `SetTrue`.  This would be a breaking change and would have to wait until v5.  The question is this change is worth it on its own and worth the user migration.  I suspect not as I feel the majority of users expect bools to be flags.  The behavior is documented, mistakes caught with a debug assert, we plan to elevate that debug assert in derives to ensure more people see it, and this is easily worked around.  As such, I lean towards closing this.  If there is a reason for us to re-evaluate, let us know!

---

_Closed by @epage on 2025-08-15 13:42_

---
