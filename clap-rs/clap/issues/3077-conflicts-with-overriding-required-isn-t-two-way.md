```yaml
number: 3077
title: "conflicts_with overriding required isn't two way"
type: issue
state: open
author: epage
labels:
  - C-bug
  - E-help-wanted
  - A-validators
  - E-easy
assignees: []
created_at: 2021-12-07T22:16:20Z
updated_at: 2022-10-17T12:32:35Z
url: https://github.com/clap-rs/clap/issues/3077
synced_at: 2026-01-12T16:14:14Z
```

# conflicts_with overriding required isn't two way

---

_@epage_

When testing the previous examples, I found I had to revert some changes.  We need to dig into this more. See https://github.com/clap-rs/clap/commit/93948cc7248926a6d2678ad3a065cabada8658a1

---
Reproduction case (will need to be turned into a test case)
```rust
use clap::{App, Arg};

fn main() {
    // Of the three argument types, flags are the most simple. Flags are simple switches which can
    // be either "on" or "off"
    //
    // clap also supports multiple occurrences of flags, the common example is "verbosity" where a
    // user could want a little information with "-v" or tons of information with "-v -v" or "-vv"
    let matches = App::new("MyApp")
        // Regular App configuration goes here...
        // We'll add a flag that represents an awesome meter...
        //
        // I'll explain each possible setting that "flags" accept. Keep in mind
        // that you DO NOT need to set each of these for every flag, only the ones
        // you want for your individual case.
        .arg(
            Arg::with_name("awesome")
                .help("turns up the awesome") // Displayed when showing help info
                .short('a') // Trigger this arg with "-a"
                .long("awesome") // Trigger this arg with "--awesome"
                .takes_value(true)
                .multiple(true) // This flag should allow multiple
                // occurrences such as "-aaa" or "-a -a"
                .requires("config") // Says, "If the user uses -a, they MUST
                // also use this other 'config' arg too"
                // Can also specify a list using
                // requires_all(Vec<&str>)
                .conflicts_with("output"), // Opposite of requires(), says "if the
                                           // user uses -a, they CANNOT use 'output'"
                                           // also has a conflicts_with_all(Vec<&str>)
                                           // and an exclusive(true)
        )
        .arg_from_usage("-c, --config=[FILE] 'sets a custom config file'")
        .arg_from_usage("<output> 'sets an output file'")
        .get_matches();

    // We can find out whether or not awesome was used
    if matches.is_present("awesome") {
        println!("Awesomeness is turned on");
    }

    // If we set the multiple option of a flag we can check how many times the user specified
    //
    // Note: if we did not specify the multiple option, and the user used "awesome" we would get
    // a 1 (no matter how many times they actually used it), or a 0 if they didn't use it at all
    match matches.occurrences_of("awesome") {
        0 => println!("Nothing is awesome"),
        1 => println!("Some things are awesome"),
        2 => println!("Lots of things are awesome"),
        _ => println!("EVERYTHING is awesome!"),
    }

    // Continued program logic goes here...
}
```
Running
```rust
$ cargo run -- --awesome --config foo
error: The following required arguments were not provided:
    <output>

USAGE:
    test-clap --config <FILE> --awesome <awesome>... <output>

For more information try --help
```

`output` is required but the conflict against it is suppose to remove that.

I double checked with clap2 and it seems to be present there as well.

`Validator::is_missing_required_ok` is most likely where this is located.  #3212 changed up the conflict code which hopefully made it easier to reuse in this function.

---

_Renamed from "co" to "conflicts_with isn't two way" by @epage on 2021-12-07 22:16_

---

_Label `T: bug` added by @epage on 2021-12-07 22:16_

---

_Comment by @pksunkara on 2021-12-08 00:38_

To give more context, I suspect that this is being caused by the shortcircuit nature of our validation where we report errors as soon as we find them and construct usage string for the errors with that information. It is probably done like this for performance reasons.

But since I haven't actually done any investigation, this might not actually end up being the reason at all.

---

_Label `A-validators` added by @epage on 2021-12-09 00:36_

---

_Comment by @epage on 2021-12-13 22:35_

We'll have to recreate a reproduction case but I believe the problem I had is that there wasn't an error being reported, so this would be independent of short-circuiting.

---

_Label `S-triage` added by @epage on 2021-12-13 22:35_

---

_Comment by @epage on 2021-12-22 19:39_

My suspicion is that the root cause is
```rust
    fn is_missing_required_ok(&self, a: &Arg<'help>, matcher: &ArgMatcher) -> bool {
        debug!("Validator::is_missing_required_ok: {}", a.name);
        self.validate_arg_conflicts(a, matcher) || self.p.overridden.contains(&a.id)
    }

    fn validate_arg_conflicts(&self, a: &Arg<'help>, matcher: &ArgMatcher) -> bool {
        debug!("Validator::validate_arg_conflicts: a={:?}", a.name);
        a.blacklist.iter().any(|conf| {
            matcher.contains(conf)
                || self
                    .p
                    .app
                    .groups
                    .iter()
                    .find(|g| g.id == *conf)
                    .map_or(false, |g| g.args.iter().any(|arg| matcher.contains(arg)))
        })
    }
```

We are only checking what the missing required arg conflicts with.  We are not checking what present args conflict with the missing required arg.  In the conflict detection code, we explicitly look for this.

Another bug in this is that we are not recursively unrolling the group like `unroll_args_in_group` does.

---

_Comment by @epage on 2021-12-23 16:53_

```rust
use clap::{App, Arg};

fn main() {
    // Of the three argument types, flags are the most simple. Flags are simple switches which can
    // be either "on" or "off"
    //
    // clap also supports multiple occurrences of flags, the common example is "verbosity" where a
    // user could want a little information with "-v" or tons of information with "-v -v" or "-vv"
    let matches = App::new("MyApp")
        // Regular App configuration goes here...
        // We'll add a flag that represents an awesome meter...
        //
        // I'll explain each possible setting that "flags" accept. Keep in mind
        // that you DO NOT need to set each of these for every flag, only the ones
        // you want for your individual case.
        .arg(
            Arg::with_name("awesome")
                .help("turns up the awesome") // Displayed when showing help info
                .short('a') // Trigger this arg with "-a"
                .long("awesome") // Trigger this arg with "--awesome"
                .takes_value(true)
                .multiple(true) // This flag should allow multiple
                // occurrences such as "-aaa" or "-a -a"
                .requires("config") // Says, "If the user uses -a, they MUST
                // also use this other 'config' arg too"
                // Can also specify a list using
                // requires_all(Vec<&str>)
                .conflicts_with("output"), // Opposite of requires(), says "if the
                                           // user uses -a, they CANNOT use 'output'"
                                           // also has a conflicts_with_all(Vec<&str>)
                                           // and an exclusive(true)
        )
        .arg_from_usage("-c, --config=[FILE] 'sets a custom config file'")
        .arg_from_usage("<output> 'sets an output file'")
        .get_matches();

    // We can find out whether or not awesome was used
    if matches.is_present("awesome") {
        println!("Awesomeness is turned on");
    }

    // If we set the multiple option of a flag we can check how many times the user specified
    //
    // Note: if we did not specify the multiple option, and the user used "awesome" we would get
    // a 1 (no matter how many times they actually used it), or a 0 if they didn't use it at all
    match matches.occurrences_of("awesome") {
        0 => println!("Nothing is awesome"),
        1 => println!("Some things are awesome"),
        2 => println!("Lots of things are awesome"),
        _ => println!("EVERYTHING is awesome!"),
    }

    // Continued program logic goes here...
}
```
Running
```rust
$ cargo run -- --awesome --config foo
error: The following required arguments were not provided:
    <output>

USAGE:
    test-clap --config <FILE> --awesome <awesome>... <output>

For more information try --help
```

`output` is required but the conflict against it is suppose to remove that.

I double checked with clap2 and it seems to be present there as well.

---

_Renamed from "conflicts_with isn't two way" to "conflicts_with overriding required isn't two way" by @epage on 2021-12-23 19:49_

---

_Referenced in [clap-rs/clap#3212](../../clap-rs/clap/pulls/3212.md) on 2021-12-23 19:52_

---

_Comment by @epage on 2021-12-23 20:35_

#3212 made the conflict code more reusable so hopefully it'll make it easier to fix `is_missing_required_ok`.

---

_Label `S-triage` removed by @epage on 2021-12-23 20:36_

---

_Label `E-easy` added by @epage on 2021-12-23 20:36_

---

_Label `E-help-wanted` added by @epage on 2022-09-20 14:32_

---

_Comment by @marcospb19 on 2022-10-17 04:13_

@epage is this a valid reproduction of this error?

```rust
use clap::Parser;

#[derive(Parser, Debug)]
pub struct Args {
    #[arg(required = true)]
    output: String,

    #[arg(long, conflicts_with = "output")]
    format: String,
}

fn main() {
    let _ = Args::parse();
}
```

```sh
$ cargo run -q -- --format .tar.gz
error: The following required argument was not provided: output

Usage: rust --format <FORMAT> <OUTPUT>

For more information try '--help'
```

Not only the usage example contains both `--format` and `<OUTPUT>`, which should be impossible, but clap also thinks that `<OUTPUT>` should be required.

---

_Comment by @epage on 2022-10-17 12:32_

@marcospb19 you are likely running into a different issue: `output` is a `String` so when `conflcits_with` overrides `required = true`, there isn't anything to fill the value in with.  You need to make it `output: Option<String>`.

---
