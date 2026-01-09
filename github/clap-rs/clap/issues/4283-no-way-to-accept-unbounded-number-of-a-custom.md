---
number: 4283
title: No way to accept unbounded number of a custom formatted values that starts with a hyphen
type: issue
state: open
author: lukehsiao
labels:
  - C-bug
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2022-09-29T03:24:00Z
updated_at: 2022-09-29T21:31:53Z
url: https://github.com/clap-rs/clap/issues/4283
synced_at: 2026-01-07T13:12:20-06:00
---

# No way to accept unbounded number of a custom formatted values that starts with a hyphen

---

_Issue opened by @lukehsiao on 2022-09-29 03:24_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.64.0 (a55dd71d5 2022-09-19)

### Clap Version

4.0.2

### Minimal reproducible code

```rust
use std::path::PathBuf;

use anyhow::{anyhow, Result};
use clap::Parser;

#[derive(Debug, clap::Parser)]
#[command(version, about)]
pub struct Opt {
    /// Input file
    #[arg(short, long)]
    pub input: Option<PathBuf>,

    /// Allow a series of potentially hyphenated values
    #[arg(required = true, allow_hyphen_values = true, num_args(1..), value_parser = choice)]
    pub choices: Vec<Choice>,
}

#[derive(Debug, Clone)]
pub struct Choice {
    pub content: String,
}

pub fn choice(src: &str) -> Result<Choice> {
    dbg!(src);

    if src.contains('i') {
        return Err(anyhow!("Why are we parsing the flag?"));
    }

    Ok(Choice {
        content: src.to_string(),
    })
}

fn main() {
    let opt = Opt::parse();
    dbg!(opt);
}
```


### Steps to reproduce the bug with the above code

```
cargo run -- -3 -i test/file.png
```

### Actual Behaviour

This is both in v3 and v4 with `Arg::allow_hyphen_values`

```
✦ ❯ cargo run -- -3 -i test/file.png
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/demo -3 -i test/file.png`
[src/main.rs:27] src = "-3"
[src/main.rs:27] src = "-i"
error: Invalid value "-i" for '<CHOICES>...': Why are we parsing the flag?

For more information try '--help'
```

### Expected Behaviour

With clap v3 **and** `Command::allow_hyphen_values`, this parsed correctly as `-3` as a positional value, and `-i test/file.png` as an option.

Also note that using `allow_negative_numbers` is not a solution here, as sometimes the values may be things like `-3:-4`.

### Additional Context

https://docs.rs/clap/latest/clap/builder/struct.Arg.html#method.allow_hyphen_values

> NOTE: If a positional argument has allow_hyphen_values and is followed by a known flag, it will be treated as a flag (see [trailing_var_arg](https://docs.rs/clap/latest/clap/builder/struct.Arg.html#method.trailing_var_arg) for consuming known flags). If an option has allow_hyphen_values and is followed by a known flag, it will be treated as the value for the option.

This issue seems to go against the documentation. Here, we have a positional argument followed by a known flag, but, the flag is NOT treated as a flag, and instead causes an unexpected parsing error.

This behavior is the same whether or not I add `trailing_var_arg = false` to the positional arg.

### Debug Output

```
❯ cargo run -- -3 -i test/file.png
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/demo -3 -i test/file.png`
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="demo"
[      clap::builder::command]  Command::_propagate:demo
[      clap::builder::command]  Command::_check_help_and_version:demo expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_check_help_and_version: Building default --version
[      clap::builder::command]  Command::_propagate_global_args:demo
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:input
[clap::builder::debug_asserts]  Arg::_debug_asserts:choices
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Arg::_debug_asserts:version
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("-3")' ([45, 51])
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("-3")
[        clap::parser::parser]  Parser::get_matches_with: sc=None
[        clap::parser::parser]  Parser::parse_short_arg: short_arg=ShortFlags { inner: RawOsStr("3"), utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['3']) }, invalid_suffix: None }
[        clap::parser::parser]  Parser::parse_short_args: positional at 1 allows hyphens
[        clap::parser::parser]  Parser::get_matches_with: After parse_short_arg MaybeHyphenValue
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("-i")' ([45, 105])
[        clap::parser::parser]  Parser::parse_short_arg: short_arg=ShortFlags { inner: RawOsStr("i"), utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['i']) }, invalid_suffix: None }
[        clap::parser::parser]  Parser::parse_short_args: prior arg accepts hyphenated values
[        clap::parser::parser]  Parser::get_matches_with: After parse_short_arg MaybeHyphenValue
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("test/file.png")' ([116, 101, 115, 116, 47, 102, 105, 108, 101, 46, 112, 110, 103])
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::resolve_pending: id="choices"
[        clap::parser::parser]  Parser::react action=Append, identifier=Some(Index), source=CommandLine
[        clap::parser::parser]  Parser::remove_overrides: id="choices"
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id="choices", source=CommandLine
[      clap::builder::command]  Command::groups_for_arg: id="choices"
[        clap::parser::parser]  Parser::push_arg_values: ["-3", "-i", "test/file.png"]
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=1
[src/main.rs:27] src = "-3"
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=2
[src/main.rs:27] src = "-i"
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
error: Invalid value "-i" for '<CHOICES>...': Why are we parsing the flag?

For more information try '--help'
```

---

_Label `C-bug` added by @lukehsiao on 2022-09-29 03:24_

---

_Referenced in [theryangeary/choose#55](../../theryangeary/choose/pulls/55.md) on 2022-09-29 03:24_

---

_Renamed from "Combining allow_hyphen_values with custom value_parser parses flags" to "Combining allow_hyphen_values with custom value_parser unepxectedly parses known flags" by @lukehsiao on 2022-09-29 03:24_

---

_Renamed from "Combining allow_hyphen_values with custom value_parser unepxectedly parses known flags" to "Combining `allow_hyphen_values` with custom `value_parser` unepxectedly parses known flags" by @lukehsiao on 2022-09-29 03:39_

---

_Renamed from "Combining `allow_hyphen_values` with custom `value_parser` unepxectedly parses known flags" to "Combining `allow_hyphen_values` with custom `value_parser` unexpectedly parses known flags" by @lukehsiao on 2022-09-29 14:24_

---

_Comment by @epage on 2022-09-29 16:46_

I've reduced the reproduction case down to:
```rust
#!/usr/bin/env -S rust-script

//! ```cargo
//! [dependencies]
//! clap = { version = "4", features = ["derive"] }
//! ```

use std::path::PathBuf;

use clap::Parser;

#[derive(Debug, clap::Parser)]
#[clap(version, about)]
pub struct Opt {
    /// Input file
    #[clap(short, long)]
    pub input: Option<PathBuf>,

    /// Allow a series of potentially hyphenated values
    #[clap(required = true, allow_hyphen_values = true)]
    pub choices: Vec<String>,
}

fn main() {
    let opt = Opt::parse();
    dbg!(opt);
}
```

---

_Comment by @epage on 2022-09-29 16:47_

Switching to clap 3 produces the same result
```rust
#!/usr/bin/env -S rust-script

//! ```cargo
//! [dependencies]
//! clap = { version = "3", features = ["derive"] }
//! ```

use std::path::PathBuf;

use clap::Parser;

#[derive(Debug, clap::Parser)]
#[clap(version, about)]
pub struct Opt {
    /// Input file
    #[clap(short, long)]
    pub input: Option<PathBuf>,

    /// Allow a series of potentially hyphenated values
    #[clap(required = true, allow_hyphen_values = true)]
    pub choices: Vec<String>,
}

fn main() {
    let opt = Opt::parse();
    dbg!(opt);
}
```
```
[clap-4283.rs:26] opt = Opt {
    input: None,
    choices: [
        "-3",
        "-i",
        "panic.rs",
    ],
}
```

However, if I switch to `Command::allow_hyphen_values`, it works as reported
```rust
#!/usr/bin/env -S rust-script

//! ```cargo
//! [dependencies]
//! clap = { version = "3", features = ["derive"] }
//! ```

use std::path::PathBuf;

use clap::Parser;

#[derive(Debug, clap::Parser)]
#[clap(version, about)]
#[clap(allow_hyphen_values = true)]
pub struct Opt {
    /// Input file
    #[clap(short, long)]
    pub input: Option<PathBuf>,

    /// Allow a series of potentially hyphenated values
    #[clap(required = true)]
    pub choices: Vec<String>,
}

fn main() {
    let opt = Opt::parse();
    dbg!(opt);
}
```
```
[clap-4283.rs:27] opt = Opt {
    input: Some(
        "panic.rs",
    ),
    choices: [
        "-3",
    ],
}
```

---

_Comment by @lukehsiao on 2022-09-29 16:51_

> However, if I switch to Command::allow_hyphen_values, it works as reported

My apologies on the incomplete report. We were actually using `#[clap(setting = clap::AppSettings::AllowLeadingHyphen)]` with clap v3, which was working. I believe this is similar to `Command::allow_hyphen_values`. Either way, your minimum reproduction case is much more minimal and shows the issue :).

(Plus, I learned about `#!/usr/bin/env -S rust-script`, so thanks!)


---

_Comment by @epage on 2022-09-29 16:52_

> My apologies on the incomplete report. We were actually using #[clap(setting = clap::AppSettings::AllowLeadingHyphen)] with clap v3, which was working. I believe this is similar to Command::allow_hyphen_values. Either way, your minimum reproduction case is much more minimal and shows the issue :).

Yes, they are the same thing.

> (Plus, I learned about #!/usr/bin/env -S rust-script, so thanks!)

I'm actually using [a fork](https://github.com/epage/cargo-script-mvs/) that exists for the purpose of [creating an RFC](https://github.com/epage/cargo-script-mvs/discussions/15)

---

_Comment by @epage on 2022-09-29 17:04_

Hmm, looks like we had a test specifically for the "broken" behavior in clap 3.  This is going to take some digging to unwind all of this
```rust
#[test]
fn leading_hyphen_with_flag_after() {
    let r = Command::new("mvae")
        .arg(
            arg!(o: -o <opt> "some opt")
                .multiple_values(true)
                .allow_hyphen_values(true),
        )
        .arg(arg!(f: -f "some flag").action(ArgAction::SetTrue))
        .try_get_matches_from(vec!["", "-o", "-2", "-f"]);
    assert!(r.is_ok(), "{}", r.unwrap_err());
    let m = r.unwrap();
    assert!(m.contains_id("o"));
    assert_eq!(
        m.get_many::<String>("o")
            .unwrap()
            .map(|v| v.as_str())
            .collect::<Vec<_>>(),
        &["-2", "-f"]
    );
    assert!(!*m.get_one::<bool>("f").expect("defaulted by clap"));
}
```

---

_Renamed from "Combining `allow_hyphen_values` with custom `value_parser` unexpectedly parses known flags" to "`allow_hyphen_values` unexpectedly parses known flags following positional arg" by @lukehsiao on 2022-09-29 17:27_

---

_Renamed from "`allow_hyphen_values` unexpectedly parses known flags following positional arg" to "`allow_hyphen_values` unexpectedly parses known flags after positional arg" by @lukehsiao on 2022-09-29 17:27_

---

_Comment by @epage on 2022-09-29 18:11_

So consuming the flag after has been the behavior since the arg-specific setting was introduced in #755

#4187 changed some of the behavior in 4.0.0 and is what is responsible for the documentation change to what is quoted in the issue.

---

_Comment by @epage on 2022-09-29 18:23_

https://github.com/BurntSushi/ripgrep/pull/233#issuecomment-261838065 discusses the motivation for the design in #755

In #4187, what I fixed was when you have the flag before the positional (`-i test/file.png -3`), it still left `-3 -i test/file.png` as-is.

---

_Comment by @epage on 2022-09-29 18:40_

In 7a2bbca62bf245970866d3de76293de435dc8dbe, we had a check for `self.cmd.is_allow_hyphen_values_set()` and if the short was known.  I think this was the logic that wasn't mirrored over into the arg-specific version, replacing the logic for checking if the last seen arg supported hyphen values.

---

_Comment by @epage on 2022-09-29 18:57_

So the rules as of today
- Prior arguments that accept hyphens/numbers get precedence over new flags
  - There is the open question "which should win when a value looks like a flag?"
  - An important reason to let the value win is with `num_args(1)` (we generally try to discourage unbounded `num_args`).  The workaround otherwise is to use an attached value, like `-fvalue` or `--flag=value`.
- Known flags get precedence over the next possible positional argument which accepts hyphens/numbers
- This is important for being able to have flags in the first place and `--` can always be used.

At the moment, I'm leaning towards this being a documentation bug rather than a behavior bug or feature request.

---

_Comment by @lukehsiao on 2022-09-29 19:10_

> At the moment, I'm leaning towards this being a documentation bug rather than a behavior bug or feature request.

This is reasonable. 

It would "break" the cli of [`choose`](https://github.com/theryangeary/choose) if they adopt clap v4, so, selfishly, I'd vote that this become a supported feature as it was possible with structopt and clap v3, but also understand not wanting to.

That said, if a documentation change turns out to be the plan, I'd suggest changing both the docs linked above on `allow_hyphen_values` and other references like https://github.com/clap-rs/clap/pull/4187/files#diff-06572a96a58dc510037d5efa622f9bec8519bc1beab13c9f251e97e657a9d4edR47 which suggest a known short flag would be treated differently then a hyphen-value.



---

_Comment by @epage on 2022-09-29 19:20_

If I'm understanding `choice`s situation, you are using negative numbers **but** there is additional syntax so the "allow negative numbers" feature doesn't work for it?

---

_Comment by @lukehsiao on 2022-09-29 19:23_

> If I'm understanding choices situation, you are using negative numbers but there is additional syntax so the "allow negative numbers" feature doesn't work for it?

Correct, they can also be things like `-3:-2`, where the colon messes things up.

---

_Comment by @epage on 2022-09-29 19:29_

Something I've been considering is generalizing `allow_negative_numbers` to asking the `ValueParser` whether the value "looks correct".

The issues to work out
- How to do this ergonomically (people don't want to always be off writing a `TypedValueParser` implementation0
- How to distinguish when this should be respected or not (an `isize` isn't too greedy but a `String` would be)
- How to mitigate any performance hit from this (especially if we always defer to the `ValueParser` without any other opt-in)

---

_Renamed from "`allow_hyphen_values` unexpectedly parses known flags after positional arg" to "No way to unbounded number of a custom format that starts with a hyphen" by @epage on 2022-09-29 19:43_

---

_Comment by @lukehsiao on 2022-09-29 19:44_

That does sound interesting!

I'm no `clap` expert, so to double check, it is not possible to get the behavior I want in clap v4 by implementing a `TypedValueParser`, right?

---

_Renamed from "No way to unbounded number of a custom format that starts with a hyphen" to "No way to accept unbounded number of a custom formatted values that starts with a hyphen" by @epage on 2022-09-29 19:44_

---

_Comment by @epage on 2022-09-29 19:50_

At the moment, it is not.  I have re-worked the issue for being focused on restoring your use case, somehow, including possibly the `TypedValueParser` route.

Another interesting angle to include in this is that when we are parsing, we try to capture up to the max for `num_args`.  If we instead capture up to the min for `num_args` and then check the `TypedValueParser` for anything else, this will
- Let us bypass asking `TypedValueParser` in most cases
- Improve error reporting for when people just typed in the syntax incorrectly in most cases

---

_Label `A-parsing` added by @epage on 2022-09-29 19:50_

---

_Label `S-waiting-on-design` added by @epage on 2022-09-29 19:50_

---

_Comment by @lukehsiao on 2022-09-29 21:31_

Got it. Thank you for the triaging and explanations :)

---

_Referenced in [clap-rs/clap#4346](../../clap-rs/clap/issues/4346.md) on 2022-10-04 16:18_

---
