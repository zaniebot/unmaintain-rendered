---
number: 2500
title: Change default help and version message to infinitive form
type: issue
state: closed
author: ajeetdsouza
labels:
  - C-enhancement
  - A-help
  - ":money_with_wings: $5"
assignees: []
created_at: 2021-05-26T19:25:22Z
updated_at: 2021-07-30T07:35:49Z
url: https://github.com/clap-rs/clap/issues/2500
synced_at: 2026-01-10T01:27:19Z
---

# Change default help and version message to infinitive form

---

_Issue opened by @ajeetdsouza on 2021-05-26 19:25_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Describe your use case

There are two problems here.

1. `clap` uses an unconventional sentence form in its auto-generated help messages (i.e. `"Prints a help message"` vs `"Print a help message"`). A lot of users (as shown in the links above) would like to change this, because practically every other UNIX program shows it in the infinitive form:

```sh
$ grep --help
-b, --byte-offset    print the byte offset with output lines

$ git init --help
--bare    create a bare repository

$ rg -h
--debug    Show debug messages.
```
2. There's no way to set this help message manually if you use `clap_derive`.

### Describe the solution you'd like

I'd like a way to set `global_help_message` and `global_version_message` in my app.
### Alternatives, if applicable

`app.mut_arg("help", |a| a.about("help info"))` is a solution, but it does not work for apps using `clap_derive`.

### Additional Context

There's plenty of demand for this feature:

- https://github.com/clap-rs/clap/issues/2080
- https://github.com/clap-rs/clap/issues/1640
- https://github.com/clap-rs/clap/issues/1646
- https://github.com/clap-rs/clap/issues/1512
- https://github.com/clap-rs/clap/issues/889
- https://github.com/clap-rs/clap/issues/1665
- https://github.com/clap-rs/clap/discussions/2152

---

_Label `T: new feature` added by @ajeetdsouza on 2021-05-26 19:25_

---

_Comment by @pksunkara on 2021-05-26 23:46_

1. We can change the default help string in clap.
2. You can use `mut_arg("help", |a| a.about("help info"))` in clap_derive. Try pasting that on a struct in clap attribute.

---

_Comment by @ajeetdsouza on 2021-05-27 05:44_

1. Would you be open to accepting a PR to change the default string?
2. Perhaps I'm doing something wrong, but I wasn't able to get this working. I'm using this on a struct with `AppSettings::DisableHelpSubcommand` and I ran it with `cargo run -- -h`:

```sh
# mut_arg("-h", |a| a.about("help info"))
USAGE:
    app [-h] <SUBCOMMAND>

ARGS:
    <-h>    help info

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
```

```sh
# mut_arg("--help", |a| a.about("help info"))
USAGE:
    zoxide [--help] <SUBCOMMAND>

ARGS:
    <--help>    help info

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
```

```sh
# mut_arg("help", |a| a.about("help info"))
error: Found argument '-h' which wasn't expected, or isn't valid in this context

If you tried to supply `-h` as a PATTERN use `-- -h`

USAGE:
    zoxide [help] <SUBCOMMAND>
```

---

_Comment by @pksunkara on 2021-05-27 09:43_

1. Yes.

2. Try master

On Thu, May 27, 2021, 06:44 ajeetdsouza ***@***.***> wrote:

>
>    1. Would you be open to accepting a PR to change the default string?
>    2. Perhaps I'm doing something wrong, but I wasn't able to get this
>    working. I'm using this on a struct with
>    AppSettings::DisableHelpSubcommand and I ran it with cargo run -- -h:
>
> # mut_arg("-h", |a| a.about("help info"))
> USAGE:
>     app [-h] <SUBCOMMAND>
>
> ARGS:
>     <-h>    help info
>
> FLAGS:
>     -h, --help       Prints help information
>     -V, --version    Prints version information
>
> # mut_arg("--help", |a| a.about("help info"))
> USAGE:
>     zoxide [--help] <SUBCOMMAND>
>
> ARGS:
>     <--help>    help info
>
> FLAGS:
>     -h, --help       Prints help information
>     -V, --version    Prints version information
>
> # mut_arg("help", |a| a.about("help info"))
> error: Found argument '-h' which wasn't expected, or isn't valid in this context
>
> If you tried to supply `-h` as a PATTERN use `-- -h`
>
> USAGE:
>     zoxide [help] <SUBCOMMAND>
>
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> <https://github.com/clap-rs/clap/issues/2500#issuecomment-849339774>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AABKU36F4BU5SUEMW2RNIZDTPXL5VANCNFSM45SW3DKA>
> .
>


---

_Renamed from "Bring back help_message and version_message" to "Change default help and version message to infinitive form" by @ajeetdsouza on 2021-05-27 11:52_

---

_Comment by @ajeetdsouza on 2021-05-27 11:56_

1. Thanks! I tried doing a simple find-and-replace for this, but a couple of tests have started failing due to whitespace issues (which are impossible to track down due to the way they're printed). Have you considered using some diff library like [`pretty_assertions`](https://crates.io/crates/pretty_assertions) so that whitespace issues can be detected more easily?
2. Just checked, this is fixed on master.

---

_Comment by @pksunkara on 2021-05-27 12:05_

Make sure that you save without formatting the `tests/help.rs` while fixing the tests and it should be okay. We have an issue with no version printing blank space that's being tracked.

---

_Comment by @ajeetdsouza on 2021-05-27 12:16_

I did save without formatting, and then I disabled removing trailing whitespaces in the settings, but VS Code simply refused to stop formatting the line. It's probably a bug. I resorted to just using `sed`:

```sh
sed -i 's/Prints version information/Print version information/g'  **/*.*
sed -i 's/Prints help information/Print help information/g'  **/*.*
sed -i 's/Prints this message or the help of the given subcommand/Print this message or the help of the given subcommand/g'  **/*.*
```

Now, a lot more tests pass, but a few are still failing due to whitespace issues. I switched to `pretty_assertions` locally to see what was going on, apparently a bunch of random whitespace is being inserted towards the end of the help string, not sure why.

---

_Comment by @pksunkara on 2021-06-01 21:49_

As I said, We have an issue with no version printing blank space that's being tracked.

If you change only the messages without any whitespace changes, it should be fixed.

---

_Label `T: new feature` removed by @pksunkara on 2021-06-01 21:50_

---

_Label `C: help message` added by @pksunkara on 2021-06-01 21:50_

---

_Label `D: easy` added by @pksunkara on 2021-06-01 21:50_

---

_Label `T: enhancement` added by @pksunkara on 2021-06-01 21:50_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-06-08 23:09_

---

_Referenced in [clap-rs/clap#2642](../../clap-rs/clap/pulls/2642.md) on 2021-07-30 03:31_

---

_Closed by @pksunkara on 2021-07-30 07:35_

---
