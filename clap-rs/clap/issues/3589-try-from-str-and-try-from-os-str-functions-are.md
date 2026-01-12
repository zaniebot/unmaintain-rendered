```yaml
number: 3589
title: try_from_str and try_from_os_str functions are called twice
type: issue
state: closed
author: aj-bagwell
labels:
  - C-bug
  - A-derive
  - S-waiting-on-mentor
assignees: []
created_at: 2022-03-29T12:42:47Z
updated_at: 2022-06-09T02:23:30Z
url: https://github.com/clap-rs/clap/issues/3589
synced_at: 2026-01-12T16:14:15Z
```

# try_from_str and try_from_os_str functions are called twice

---

_@aj-bagwell_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.58.1 (db9d1b20b 2022-01-20)

### Clap Version

3.1.6

### Minimal reproducible code

```rust
use clap::Parser;
use std::sync::atomic::{AtomicU8, Ordering};

static COUNT: AtomicU8 = AtomicU8::new(0);

#[derive(Parser)]
#[clap(name = "from_str")]
struct Opt {
    #[clap(parse(try_from_str = spy))]
    s: String,
}

fn main() {
    let opt = Opt::parse_from(&["foo", "h"]);
    assert_eq!(COUNT.load(Ordering::SeqCst), 1);
}

fn spy(s: &str) -> Result<String, String> {
    let c = COUNT.fetch_add(1, Ordering::SeqCst);
    println!("spy called {}", c);
    Ok(s.to_string())
}
```


### Steps to reproduce the bug with the above code

```cargo run```

### Actual Behaviour

When specifying a parser for an attribute using `try_from_str` and the `#[derive(Parser)]` macro the function passed used for parising is called twice, once as part of the validate phase and then again to convert the argument to the expected type.

This causes issues when the function is not idemponent, such as opening a file, or performing a network request.

### Expected Behaviour

The `try_from_str` should only be called once.

### Additional Context

_No response_

### Debug Output

```
[        clap::build::command] 	App::_do_parse
[        clap::build::command] 	App::_build
[        clap::build::command] 	App::_propagate:from_str
[        clap::build::command] 	App::_check_help_and_version: from_str
[        clap::build::command] 	App::_check_help_and_version: Removing generated version
[        clap::build::command] 	App::_propagate_global_args:from_str
[        clap::build::command] 	App::_derive_display_order:from_str
[  clap::build::debug_asserts] 	Command::_debug_asserts
[  clap::build::debug_asserts] 	Arg::_debug_asserts:help
[  clap::build::debug_asserts] 	Arg::_debug_asserts:s
[  clap::build::debug_asserts] 	Command::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("h")' ([104])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("h")
[         clap::parse::parser] 	Parser::get_matches_with: sc=None
[         clap::parse::parser] 	Parser::remove_overrides: id=s
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_arg: id=s
[        clap::build::command] 	App::groups_for_arg: id=s
[         clap::parse::parser] 	Parser::add_val_to_arg; arg=s, val=RawOsStr("h")
[         clap::parse::parser] 	Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."h"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=1
[        clap::build::command] 	App::groups_for_arg: id=s
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: o=s
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[         clap::parse::parser] 	Parser::add_defaults:iter:s:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:s: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:s: doesn't have default missing vals
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::validate_exclusive:iter:s
[      clap::parse::validator] 	Validator::validate_conflicts::iter: id=s
[      clap::parse::validator] 	Conflicts::gather_conflicts
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([Child { id: s, children: [] }])
[      clap::parse::validator] 	Validator::gather_requires
[      clap::parse::validator] 	Validator::gather_requires:iter:s
[      clap::parse::validator] 	Validator::validate_required_unless
[      clap::parse::validator] 	Validator::validate_matched_args
[      clap::parse::validator] 	Validator::validate_matched_args:iter:s: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "h",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="s"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
spy called 0
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "s"=1
[    clap::parse::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[help]
spy called 1
thread 'main' panicked at 'assertion failed: `(left == right)`
  left: `2`,
 right: `1`', src/main.rs:15:5
 ```

---

_Label `C-bug` added by @aj-bagwell on 2022-03-29 12:42_

---

_Comment by @aj-bagwell on 2022-03-29 12:46_

I think that fixing this should be as simple as just never adding the validators, if there is an error in the get phase it is beautifully printed in exactly the same way as the validator would. 

---

_Label `A-derive` added by @epage on 2022-03-29 14:27_

---

_Label `S-waiting-on-mentor` added by @epage on 2022-03-29 14:27_

---

_Referenced in [clap-rs/clap#2683](../../clap-rs/clap/issues/2683.md) on 2022-03-29 14:28_

---

_Comment by @epage on 2022-03-29 14:30_

Huh, we had a previous PR for this but no issue.  https://github.com/clap-rs/clap/issues/2683 is the tracking issue for resolving this

> I think that fixing this should be as simple as just never adding the validators, if there is an error in the get phase it is beautifully printed in exactly the same way as the validator would.

There are subtle differences, like color support.

---

_Comment by @aj-bagwell on 2022-03-29 15:55_

Bah, you are of course right it is not as simple as I thought, 
with validator (pretty colours for the `<S>` and `"/tmp/not-found"`):
```
error: Invalid value "/tmp/not-found" for '<S>': No such file or directory (os error 2)

For more information try --help
```
without validator (only `error:` gets color):
```
error: Invalid value for s: No such file or directory (os error 2)

USAGE:
    foo [OPTIONS] <S>
```

Since the nice error message relies on the `context` which can't be set via the public API, I don't think it is worth adding the colours, but adding the value is easy.

Would you be interested in a PR dropping the validate calls or should I just wait for 4.0?


---

_Comment by @epage on 2022-03-29 18:45_

>  I don't think it is worth adding the colours,

I would consider that a blocker for a solution

> Would you be interested in a PR dropping the validate calls 

Even if the final message ends up looking exactly the same, this would be borderline for breaking compatibility due to changes in expectations of behavior.  Let's just wait for that 4.0 work to go through.

---

_Comment by @aj-bagwell on 2022-03-29 19:19_

4.0 it is then! Thank you for all your hard work and speedy responses.

---

_Referenced in [clap-rs/clap#3732](../../clap-rs/clap/pulls/3732.md) on 2022-05-17 21:47_

---

_Referenced in [clap-rs/clap#3734](../../clap-rs/clap/issues/3734.md) on 2022-05-18 00:23_

---

_Referenced in [clap-rs/clap#3742](../../clap-rs/clap/pulls/3742.md) on 2022-05-23 12:26_

---

_Closed by @epage on 2022-05-23 15:33_

---

_Comment by @wiktor-k on 2022-05-26 07:59_

I've taken a look at the new way of parsing with `#[clap(value_parser)]` and it looks very nice! :clap: 

One question though: now the structs to be parsed need to `impl Clone` even though they are not cloned by the value parser. Is this intentional?

I wanted to parse the object and create it only once and intentionally make it non-clone'able since it owns some resources that should not be shared (ie. file descriptors). So it would be really nice if the must-impl-Clone restriction was lifted...

An example:

```rust
use std::convert::TryFrom;

#[derive(Debug)]
struct Xing;

impl Clone for Xing { // this is needed by value_parser
    fn clone(&self) -> Self {
        todo!() // this is not called
    }
}


impl std::str::FromStr for Xing { // FromStr needs to be implemented
    type Err = std::io::Error;

    fn from_str(_s: &str) -> Result<Self, Self::Err> {
        eprintln!("Calling..."); // this is printed only once -> nice!
        Ok(Xing)
    }
}

use clap::Parser;

#[derive(Parser, Debug)]
struct Args {
    #[clap(short, long, value_parser)] // this is new
    x: Xing,
}

fn main() {
    let args = Args::parse();
    println!("Hello, world: {:?}", args);
}
```

---

_Comment by @epage on 2022-05-26 12:04_

> One question though: now the structs to be parsed need to impl Clone even though they are not cloned by the value parser. Is this intentional?

The derive API is built on top of the builder API.  The parser generates data into an `ArgMatches` which implements `Clone`, so the data needs to be `Clone`.  Technically, we cheat right now and wrap the data in an `Arc` because I couldn't quite get it working right but (1) I want the implementation flexibility to remove the `Arc` later and (2) if the `Arc`s ref count is greater than 1, we can't move out of it and have to clone instead.

Feel free to create an issue for this if you want.

An idea I'm playing with for the future is to have `ArgMatches` implement a trait and allow alternative implementations to be used.  The main intent would be for people to organize the values in a different way than clap currently does (e.g. easier handling of order-dependent flags) but it could potentially allow a more optimized implementation for the derive API that has fewer restrictions (just `Any + 'static`, removing `Clone`, `Send`, and `Sync`).  This is still a very immature idea and I don't know how much is feasible and there are a lot of other priorities (right now, implementing Actions to fully deprecate `parse`).

At one point, `ArgMatches` exposed the `Arc` and we did the move-out-of-arc-or-clone there.  I had considered adding Deref specialization to it so we could clone where possible and panic otherwise.  I removed the `Arc` from `ArgMatches` to make the API cleaner and to give implementation flexibility (#3747).

However, I suspect most exclusive resources are not safe to acquire at process start regardless of what happens for the rest of the program and that not needing `Clone` is a corner case.  For example, a file-write handle would cause files to be created even if everything else fails.  On Windows, even file reads can be a problem because of their semantics.  This is why I have not been too worried about the lack of `Clone`.  Users can always acquire the resource later.

---

_Comment by @wiktor-k on 2022-05-27 10:22_

Thanks for the detailed explanation and your work on clap in general!

What I'm trying to achieve here is constructing my own objects from command line parameters only once.

Previously this was not the case (the objects were constructed twice: once during validation). With `value_parser` the situation is improved as the object is created only once but the added constraint of being `Clone`able, even if currently not used, still doesn't promise that the objects will be created only once (since creating object through `from_str` is not much different from cloning another one from the first one).

So, while technically the issue here is solved (`from_str` is not called twice now) the underlying intent I had (objects created only once) is not.

> Feel free to create an issue for this if you want.

I'm not sure if that's only me and I don't want to put more work on your plate if this is a rare edge-case but I still wanted to explain my rationale.

Thank you for your time!

---

_Referenced in [clap-rs/clap#3792](../../clap-rs/clap/issues/3792.md) on 2022-06-06 14:23_

---

_Comment by @epage on 2022-06-09 02:23_

@wiktor-k what are your thoughts on adding a `T: PartialEq` bound to our adapter from `FromStr`?  Users could still manually implement `TypedValueParser` to workaround it.

Right now we have a `default_value_if_eq` that compares raw values and supporting comparators in `TypedValueParser` could possibly be a way for us to support a version of `default_value_if_eq` that uses native types for the comparison.  See https://github.com/clap-rs/clap/issues/3792#issuecomment-1147834606

---

_Referenced in [aj-bagwell/clio#4](../../aj-bagwell/clio/issues/4.md) on 2022-06-22 09:07_

---
