---
number: 89
title: Possibly incorrect generated usage grammar
type: issue
state: closed
author: Byron
labels:
  - C-enhancement
assignees: []
created_at: 2015-04-30T07:00:59Z
updated_at: 2018-08-02T03:29:39Z
url: https://github.com/clap-rs/clap/issues/89
synced_at: 2026-01-10T01:26:23Z
---

# Possibly incorrect generated usage grammar

---

_Issue opened by @Byron on 2015-04-30 07:00_

When specifying an argument like this:

``` Rust
Arg::with_name("url")
    .long("scope")
    .help("Specify the authentication a method should be executed in. Each scope requires the user to grant this application permission to use it.If unset, it defaults to the shortest scope url for a particular method.")
    .multiple(true)
    .takes_value(true))
```

The auto-generated grammar shows up like this `--scope <url>...`:

``` bash
groupsmigration1 0.2.0
Sebastian Thiel <byronimo@gmail.com>
Groups Migration Api.

USAGE:
    groupsmigration1 [FLAGS] [OPTIONS] [SUBCOMMANDS]

FLAGS:
        --debug         Output all server communication to standard error. `tx` and `rx` are placed into the same stream.
        --debug-auth    Output all communication related to authentication to standard error. `tx` and `rx` are placed into the same stream.
    -h, --help          Prints help information
    -v, --version       Prints version information

OPTIONS:
        --config-dir <folder>    A directory into which we will store our persistent data. Defaults to a user-writable directory that we will create during the first invocation.[default: ~/.google-service-cli
        --scopes <url>...        Specify the authentication a method should be executed in. Each scope requires the user to grant this application permission to use it.If unset, it defaults to the shortest scope url for a particular method.

SUBCOMMANDS:
    archive
    help       Prints this message

All documentation details can be found at http://byron.github.io/google-apis-rs/google_groupsmigration1_cli
```

This grammar was used similarly by `docopt`, but meant you could call it like `program --scope url1 url2`. However, with `clap` it only works if the flag is repeated: `program --scope url1 --scope url2` which should be resulting in a different grammar, more along the lines of `[--scope <url>]...`.

I believe this inconsistency should be reviewed, either docopt has an issue there, or clap :).
## Side note

In any case, it would be great if the expected number of arguments could be `+` (_one or more_) as I also have the case where I specify `-r <value>...`, which previously allowed me to make calls like `-r v1 v2 v3 v4`, which is very convenient. With `clap`, I will now have to write `-r v1 -r v2 -r v3 -r v4`, resulting in much more noise.
This one might be related to #88, which requests a fixed amount of arguments. When thinking about python's `argparse` module, I remember that they have the notion of `num_args`, which is either `+`, `*` or an explicit number. Implementing it this way would probably be a breaking change as `takes_value(true|false)` would be more like `takes_value(Values::ZeroOrMore|Values::OneOrMore|Values::Exactly(5)|Values::None)`.


---

_Comment by @kbknapp on 2015-04-30 13:13_

Yeah, I think this is kind of related to #88. The way I plan on addressing this issue is to allow `-r <value> <value> <value> [...]` which would be short-hand for `-r <value> -r <value> -r <value>` and the usage string would still parse as `-r <value>...`

Then more specifically for #88, I'm working on a say similiar to how to how you stated `OneOrMore|Exactly` perhaps with names, in which case the usage string would parse `[-r <val1> <val2>]` etc.


---

_Label `enhancement` added by @kbknapp on 2015-04-30 13:13_

---

_Comment by @Byron on 2015-04-30 14:42_

Even better ! I am looking forward to trying it. Once that feature is implemented, along with better 'requires' error messages, the command-line parsing capabilities of all 78 google service CLIs have officially arrived.


---

_Comment by @kbknapp on 2015-04-30 23:40_

This should be good with #91 

You'll now be able to use the shorthand `-r value value value` as sugar for `-r value -r value -r value`, no configuration change needed. Now for your particular use case, you can also add names to the values, so that in the help/usage strings show something like `-r <file> <mode> <etc>`. As detailed in the docs,  the names are cosmetic only for the help/usage/error messages. They are still accessed via the `.values_of("arg")`, but they are stored in order, so for the example above, the first value is `file`, the second value is `mode`, etc.

The one other thing this PR includes is a `min_values()`, `max_values()`, and `number_of_values()` to specify the min, max, and exact number of "values" required to satisfy an argument. Let me know if you find any issues, as this is a rather large change and although all my tests pass, it's always possible to miss something!


---

_Closed by @kbknapp on 2015-04-30 23:41_

---

_Referenced in [clap-rs/clap#88](../../clap-rs/clap/issues/88.md) on 2015-05-01 02:25_

---
