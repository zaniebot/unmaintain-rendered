---
number: 5361
title: Multi valued flags break collapsing short flags
type: issue
state: closed
author: BenWiederhake
labels:
  - C-bug
  - S-waiting-on-decision
  - A-parsing
assignees: []
created_at: 2024-02-17T12:40:41Z
updated_at: 2024-02-20T01:40:47Z
url: https://github.com/clap-rs/clap/issues/5361
synced_at: 2026-01-07T13:12:20-06:00
---

# Multi valued flags break collapsing short flags

---

_Issue opened by @BenWiederhake on 2024-02-17 12:40_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.76.0 (07dca489a 2024-02-04)

### Clap Version

4.5.1

### Minimal reproducible code

```rust
use clap::{Arg, ArgAction, Command};
pub fn make_command() -> Command {
    Command::new("ouch")
        .arg(Arg::new("echo").short('e').num_args(0..))
        .arg(Arg::new("zero-terminated").short('z').action(ArgAction::SetTrue))
}
fn main() {
    let testcases = [
        // -e intentionally takes zero..many arguments:
        ("ouch -e", (vec![], false)),
        ("ouch -e asdf", (vec!["asdf"], false)),
        ("ouch -e foo bar", (vec!["foo", "bar"], false)),
        // When -e and -z are passed separately, it works well:
        ("ouch -e -z", (vec![], true)),
        ("ouch -e asdf -z", (vec!["asdf"], true)),
        ("ouch -e foo bar -z", (vec!["foo", "bar"], true)),
        ("ouch -z -e", (vec![], true)),
        ("ouch -z -e asdf", (vec!["asdf"], true)),
        ("ouch -z -e foo bar", (vec!["foo", "bar"], true)),
        // When -z goes first, there's no issue either:
        ("ouch -ze", (vec![], true)),
        ("ouch -ze asdf", (vec!["asdf"], true)),
        ("ouch -ze foo bar", (vec!["foo", "bar"], true)),
        // When they are passed together, all hell breaks loose:
        ("ouch -ez", (vec![], true)),
        ("ouch -ez asdf", (vec!["asdf"], true)),
        ("ouch -ez foo bar", (vec!["foo", "bar"], true)),
    ];
    for case in testcases {
        let (inputline, expected_result) = case;
        println!("input: {inputline}");
        match make_command().try_get_matches_from(inputline.split_whitespace()) {
            Ok(result) => {
                let echo_args: Vec<String> = result.get_many::<String>("echo").unwrap().map(String::from).collect();
                let actual_result = (echo_args, result.get_flag("zero-terminated"));
                println!("  expected: {expected_result:?}");
                println!("  actual:   {actual_result:?}");
                if expected_result.0 != actual_result.0 || expected_result.1 != actual_result.1 {
                    println!("  FAIL! <========");
                } else {
                    println!("  GOOD");
                }
            }
            Err(err) => {
                println!("Parsing failed?! {err:?}");
            }
        }
    }
}
```

### Steps to reproduce the bug with the above code

The above code demonstrates several test cases. Among them is the problematic case `-ez foo bar`, where `z` is supposed to be interpreted as the noarg flag `--zero-terminated`. Note that behavior like this is sometimes desirable, e.g. when emulating GNU shuf:

```console
$ gnushuf -ez foo bar baz | hd
00000000  62 61 72 00 66 6f 6f 00  62 61 7a 00              |bar.foo.baz.|
0000000c
```

### Actual Behaviour

Clap interprets the `z` in `-ez foo bar` as the sole argument to `-e`, as if there was a call to `value_delimiter`. clap then errors, and refuses to understand the `foo bar` part of the command.

### Expected Behaviour

Clap should (provide a switch to enable a mode in order to) prefer short-arg expansion over argument-taking. In other words, I want `-ez foo bar` to be interpreted as the two separate concepts `--echo foo bar` and `--zero-terminated`.

### Additional Context

I understand that this might be considered a breaking change. I can see at least two ways to go about this:
1. Make it opt-in on the `Command`-level, i.e. `Command::single_dash_is_always_train_of_short_args(true)` or something like that, so that *all* agglomerations of short flags are switched here.
2. Make it opt-in on the `Arg`-level, i.e. `Arg::permit_direct_short_arg(false)` or something like that, so that `-eARG` is not even considered.

I admit that this may not be a "bug" in the "crash" sense, but it's definitely an incompatibility of existing features.

### Debug Output

_No response_

### Similar but *different* issues:

- #1125: This bug does not involve required arguments
- #5115: This bug does not involve positional arguments


---

_Label `C-bug` added by @BenWiederhake on 2024-02-17 12:40_

---

_Label `A-parsing` added by @epage on 2024-02-19 18:46_

---

_Label `S-waiting-on-decision` added by @epage on 2024-02-19 18:46_

---

_Comment by @epage on 2024-02-19 18:53_

`-ez foo bar` takes the `z` as the argument and assumes that an attached value and unattached values are not mixed, and stops parsing `-e`.

Without other context, this feels niche enough that it wouldn't make sense to move forward on.  As a user, I would be confused to see `-ez foo bar` and for that to be parsed the same as `-ze foo bar`.  This would be complicated to implement, deferring the evaluation of short arguments while processing other arguments, which comes at the cost of binary size and compile times for all users who don't want this behavior.  This would also has a cost in contributing to API bloat.  Every new knob we add makes it harder to find every other knob that exists, including what we just added.  The more we add, the less likely people will use any of it.

---

_Referenced in [uutils/coreutils#4254](../../uutils/coreutils/issues/4254.md) on 2024-02-19 21:54_

---

_Comment by @BenWiederhake on 2024-02-20 01:40_

Turns out, I wanted a boolean flag all along, and clap easily supports it. Sorry for the noise, and thanks for the quick response!

---

_Closed by @BenWiederhake on 2024-02-20 01:40_

---
