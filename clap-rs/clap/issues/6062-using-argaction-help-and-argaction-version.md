```yaml
number: 6062
title: "Using `ArgAction::Help` and `ArgAction::Version` disables `conflicts_with` and `exclusive`"
type: issue
state: open
author: ntrypoint
labels:
  - C-bug
  - M-breaking-change
  - A-help
  - A-parsing
  - S-triage
assignees: []
created_at: 2025-07-07T13:13:18Z
updated_at: 2025-07-07T20:01:56Z
url: https://github.com/clap-rs/clap/issues/6062
synced_at: 2026-01-12T16:14:17Z
```

# Using `ArgAction::Help` and `ArgAction::Version` disables `conflicts_with` and `exclusive`

---

_@ntrypoint_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.88.0 (6b00bc388 2025-06-23)

### Clap Version

4.5.40

### Minimal reproducible code

main.rs:

```rust
#[allow(dead_code)]

use clap::{Arg, ArgAction, Command};

fn clap_parse() {
    let matches: clap::ArgMatches = Command::new("goto")
        .about("Jump between known directories")
        .author("My Name")
        .version(clap::crate_version!())

        // Because its not possible with clap to have
        //  goto [global options] [subcommand] [subcommand options] [positional args]
        // (at least from my understanding), use this structure:
        //  goto [subcommand] [global options + subcommand options] [positional args]
        .args_conflicts_with_subcommands(true)

        // I disable the auto-supplied stuff here because I want
        // to make help and version conflict with any other arguments
        // and need to know their ID (= create them) to do that
        .disable_help_subcommand(true)
        .disable_help_flag(true)
        .disable_version_flag(true)

        // The user can either use one of these subcommands ...
        .subcommand(
            Command::new("learn")
                .visible_alias("l")
                .arg(Arg::new("learn_dirs").num_args(1..).value_name("dir")),
        )
        .subcommand(
            Command::new("forget")
                .visible_alias("f")
                .arg(Arg::new("forget_dirs").num_args(1..).value_name("dir")),
        )
        .subcommand(
            Command::new("match")
                .visible_alias("m")
                .arg(Arg::new("match_dirs").num_args(1..).value_name("dir")),
        )
        .subcommand(
            Command::new("edit")
                .visible_alias("e")
                .arg(Arg::new("edit_dirs").num_args(1..).value_name("dir")),
        )

        // ... or no subcommand, which tries to change directory to "target".
        .arg(
            Arg::new("target")
                .num_args(1)
                .value_name("target")
                .conflicts_with_all(["help", "version"])
                .help("The query string to jump to"),
        )

        // These boolean/value flags should work in all cases:
        .arg(
            Arg::new("cwd")
                .global(true)
                .num_args(1)
                .short('C')
                .long("working-dir")
                .value_name("dir")
                .help("Work from this directory"),
        )
        .arg(
            Arg::new("verbose")
                .global(true)
                .short('v')
                .long("verbose")
                .action(ArgAction::SetTrue)
                .help("Print extra information"),
        )

        // Using --version should only be possible like this:
        //   goto --version <--no trailing stuff here-->
        .arg(
            Arg::new("version")
                .exclusive(true)
                .short('V')
                .long("version")
                .action(ArgAction::Version)
                .help("Print the version"),
        )

        // Using --help should only be possible like this:
        //   goto --help <--no trailing stuff here-->
        //   (and if possible also)  goto <subcommand> --help <--no trailing stuff here-->
        // Although that is a little silly since it would probably be
        // ok to display the help anyway. I'd prefer the message
        // saying the combination of arguments is illegal still.
        .arg(
            Arg::new("help")
                .exclusive(true)
                .short('h')
                .long("help")
                .action(ArgAction::Help)
                .help("Print the version"),
        )
        .get_matches();

    println!("Debug output: {:?}", matches);

    if matches.get_flag("verbose") {
        println!("verbose mode active");
    }
}

fn main() {
    clap_parse();
    println!("Hello, world!");
}
```


### Steps to reproduce the bug with the above code

1. Compile and run the above code to make sure it works
2. Run the compiled binary with arguments `cargo run -- foo --help` or `cargo run -- foo --version`

### Actual Behaviour

 I observe that these inputs display the help / version just fine even though they should lead to an error based on what the builder code here reads. Something like `goto Documents --version` should just be an error, but it seems the `target` argument is assigned the string "Documents" and then the ´ArgAction::Version´ runs, even though `--version` is supposed to be exclusive.

### Expected Behaviour

There should be a runtime error along the lines of `error: the argument ... cannot be used with one or more of the other specified arguments`.

### Additional Context

I am aware that running help/version terminates early. However I still think this behaviour is confusing and wrong. Replacing the two `ArgAction`s with the `SetTrue` action, everything just works as intended. But I would very much like to use the automatic help / version display... ☹️

### Debug Output

_No response_

---

_Label `C-bug` added by @ntrypoint on 2025-07-07 13:13_

---

_Comment by @epage on 2025-07-07 14:25_

Could you explain the context for why you want to setup these conflicts?

There are two parts to this behavior:
- The mechanism: `ArgAction`s are what happens when you encounter an argument and are processed before conflicts or overrides
- The policy: this ensures someone can always append the help to any command and see it, rather than having to edit the command down to what works

---

_Label `A-help` added by @epage on 2025-07-07 14:25_

---

_Label `A-parsing` added by @epage on 2025-07-07 14:25_

---

_Label `S-triage` added by @epage on 2025-07-07 14:26_

---

_Comment by @ntrypoint on 2025-07-07 19:51_

> Could you explain the context for why you want to setup these conflicts?

There is not a lot of context here, this is my first Rust project and first time using this (pretty neat) library. The reason I thought something was broken is simply because

```
$ ./my_program <other args...> --version
```

looks to me like it should be an error, because it is semantically ambiguous/wrong. And looking at the code, it seems this is precisely the thing being forbidden. Should the command do its thing or the version be printed?

Maybe I'm using clap wrong. Is there a way to print the version information and help using clap's facilities manually? Then I might just do that.

---

_Comment by @epage on 2025-07-07 20:01_

`--version` is a little but different than `--help` though I imagine being able to append it could still be useful for a bug report.  I would be open to considering changing `--version` though us making that breaking change would then be a breaking change in all applications that use `clap`.

For `--version`, we just print back the information you give us (though for the derive, we do take care of the `env!("CARGO_PKG_VERSION")` for you).  You can call [`Command::render_version`](https://docs.rs/clap/latest/clap/struct.Command.html#method.render_version) (and from the derive you can get a `Command` via the [`CommandFactory`](https://docs.rs/clap/latest/clap/trait.CommandFactory.html) trait that is `impl`ed for you).

For `--help`, I would again advice against changing this as appending `--help` to any existing command is a very beneficial feature for your users.  If you still insist, then you can call `Command::render_help`.

---

_Label `M-breaking-change` added by @epage on 2025-07-07 20:01_

---
