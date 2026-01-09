---
number: 5355
title: "Don't show global options"
type: issue
state: open
author: nwalfield
labels:
  - C-enhancement
assignees: []
created_at: 2024-02-16T12:50:40Z
updated_at: 2024-02-21T15:15:03Z
url: https://github.com/clap-rs/clap/issues/5355
synced_at: 2026-01-07T13:12:20-06:00
---

# Don't show global options

---

_Issue opened by @nwalfield on 2024-02-16 12:50_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.0

### Describe your use case

[`sq`](https://gitlab.com/sequoia-pgp/sequoia-sq), the command-line frontend for [sequoia-openpgp](https://gitlab.com/sequoia-pgp/sequoia), has a number of global options.  These are shown in the help output of all subcommands, which makes the `--help` output of the subcommands hard to read.  In particular, the salient information is hidden.  Consider:

```
$ sq cert --help
Manages certificates

We use the term "certificate", or "cert" for short, to refer to
OpenPGP keys that do not contain secrets.  This subcommand provides
primitives to generate and otherwise manipulate certs.

Conversely, we use the term "key" to refer to OpenPGP keys that do
contain secrets.  See `sq key` for operations on keys.

Usage: sq cert [OPTIONS] <COMMAND>

Commands:
  import  Imports certificates into the local certificate store
  export  Exports certificates from the local certificate store
  lint    Checks certificates for issues

Options:
      --cert-store <PATH>
          Specifies the location of the certificate store.  By default, sq uses the
          OpenPGP certificate directory at `$HOME/.local/share/pgp.cert.d`, and
          creates it if it does not exist.
          
          [env: SQ_CERT_STORE=]

  -f, --force
          Overwrites existing files

      --keyring <PATH>
          Specifies the location of a keyring to use.  Keyrings are used in addition
          to any certificate store.  The content of the keyring is not imported into
          the certificate store.  When a certificate is looked up, it is looked up in
          all keyrings and any certificate store, and the results are merged together.

      --known-notation <NOTATION>
          Adds NOTATION to the list of known notations. This is used when validating
          signatures. Signatures that have unknown notations with the critical bit set
          are considered invalid.

      --no-cert-store
          Disables the use of a certificate store.  Normally sq uses the user's
          standard cert-d, which is located in `$HOME/.local/share/pgp.cert.d`.

      --output-format <FORMAT>
          Produces output in FORMAT, if possible
          
          [env: SQ_OUTPUT_FORMAT=]
          [default: human-readable]

          Possible values:
          - human-readable: Output that is meant to be read by humans, instead of
            programs
          - json:           Output as JSON

      --output-version <VERSION>
          Produces output variant VERSION, such as 0.0.0. The default is the newest
          version. The output version is separate from the version of the sq program.
          To see the current supported versions, use output-versions subcommand.
          
          [env: SQ_OUTPUT_VERSION=]

      --pep-cert-store <PATH>
          Specifies the location of a pEp certificate store.  sq does not use a pEp
          certificate store by default; it must be explicitly enabled using this
          argument or the corresponding environment variable, PEP_CERT_STORE.  The pEp
          Engine's default certificate store is at `$HOME/.pEp/keys.db`.
          
          [env: PEP_CERT_STORE=]

      --time <TIME>
          Sets the reference time as an ISO 8601 formatted timestamp.  Normally,
          commands use the current time as the reference time.  This argument allows
          the user to use a difference reference time.  For instance, when creating a
          key using `sq key generate`, the creation time is normally set to the
          current time, but can be overridden using this option.  Similarly, when
          verifying a message, the message is verified with respect to the current
          time.  This option allows the user to use a different time.
          
          TIME is interpreted as an ISO 8601 timestamp.  To set the certification time
          to July 21, 2013 at midnight UTC, you can do:
          
          $ sq --time 20130721 verify msg.pgp
          
          To include a time, say 5:50 AM, add a T, the time and optionally the
          timezone (the default timezone is UTC):
          
          $ sq --time 20130721T0550+0200 verify msg.pgp

      --trust-root <FINGERPRINT|KEYID>
          Considers the specified certificate to be a trust root. Trust roots are used
          by trust models, e.g., the Web of Trust, to authenticate certificates and
          User IDs.

  -v, --verbose
          Be more verbose.

  -h, --help
          Print help (see a summary with '-h')

```

`sq cert` has one option, `--help`; all of the other options are global options.

We'd like the output of `--help` to make it easier for readers to find the most relevant information.

### Describe the solution you'd like

One idea we had is that clap could provide a mechanism to have `--help` only show local arguments, and perhaps mention that global arguments are described in the top-level `--help` output.  Doing this, the above output would be changed to something like:

```
$ sq cert --help
Manages certificates

We use the term "certificate", or "cert" for short, to refer to
OpenPGP keys that do not contain secrets.  This subcommand provides
primitives to generate and otherwise manipulate certs.

Conversely, we use the term "key" to refer to OpenPGP keys that do
contain secrets.  See `sq key` for operations on keys.

Usage: sq cert [OPTIONS] <COMMAND>

Commands:
  import  Imports certificates into the local certificate store
  export  Exports certificates from the local certificate store
  lint    Checks certificates for issues

Options:

  -h, --help
          Print help (see a summary with '-h')

Global options:

  See "sq --help" for a description of the global options.
```

from which we think it is easier to extract the most important information.

I see several two ways to add support for this to `clap`:

  - There could be an option for `Command`s similar to [`Command::hide_possible_values`](https://docs.rs/clap/latest/clap/struct.Command.html#method.hide_possible_values), perhaps `Command::hide_non_local_arguments`.

  - [`Command::help_template`](https://docs.rs/clap/latest/clap/struct.Command.html#method.help_template) could distinguish between local and global options.  That is, in addition to `{options}`, which currently shows the local and global options, there could be `{local-options}`, `{global-options}`, and `{see-global-options}`.  The third would expand to the empty string if there are no global options and otherwise to a section, as above.



### Alternatives, if applicable

Another way to hide the global options in `--help` would be to change the global options into top-level options.  This changes the CLI, but may be worth it.

We could also have a struct for all of the global options in which each argument is [hidden](https://docs.rs/clap/latest/clap/struct.Arg.html#method.hide), and then manually include and flatten it into each subcommand.  The disadvantage is that it results in a bit of code duplication.


### Additional Context

_No response_

---

_Label `C-enhancement` added by @nwalfield on 2024-02-16 12:50_

---

_Comment by @epage on 2024-02-16 13:27_

If the argument truly is global, the user needs some kind of indication that it is supported on the command.  You mention having a pointer to users about this.  This is somewhat like #5354 where the approach is we give developer the option to not show something and the developer is responsible for adding the `after_help`.

Another option not mentioned is to put the globals under their own `help_heading` and maybe even set a `display_order` on them (`next_display_order` can help) so that they appear last.

You'd then get
```
$ sq cert --help
Manages certificates

We use the term "certificate", or "cert" for short, to refer to
OpenPGP keys that do not contain secrets.  This subcommand provides
primitives to generate and otherwise manipulate certs.

Conversely, we use the term "key" to refer to OpenPGP keys that do
contain secrets.  See `sq key` for operations on keys.

Usage: sq cert [OPTIONS] <COMMAND>

Commands:
  import  Imports certificates into the local certificate store
  export  Exports certificates from the local certificate store
  lint    Checks certificates for issues

Options:
  -h, --help
          Print help (see a summary with '-h')

Global options:
      --cert-store <PATH>
          Specifies the location of the certificate store.  By default, sq uses the
          OpenPGP certificate directory at `$HOME/.local/share/pgp.cert.d`, and
          creates it if it does not exist.
          
          [env: SQ_CERT_STORE=]

  -f, --force
          Overwrites existing files

      --keyring <PATH>
          Specifies the location of a keyring to use.  Keyrings are used in addition
          to any certificate store.  The content of the keyring is not imported into
          the certificate store.  When a certificate is looked up, it is looked up in
          all keyrings and any certificate store, and the results are merged together.

      --known-notation <NOTATION>
          Adds NOTATION to the list of known notations. This is used when validating
          signatures. Signatures that have unknown notations with the critical bit set
          are considered invalid.

      --no-cert-store
          Disables the use of a certificate store.  Normally sq uses the user's
          standard cert-d, which is located in `$HOME/.local/share/pgp.cert.d`.

      --output-format <FORMAT>
          Produces output in FORMAT, if possible
          
          [env: SQ_OUTPUT_FORMAT=]
          [default: human-readable]

          Possible values:
          - human-readable: Output that is meant to be read by humans, instead of
            programs
          - json:           Output as JSON

      --output-version <VERSION>
          Produces output variant VERSION, such as 0.0.0. The default is the newest
          version. The output version is separate from the version of the sq program.
          To see the current supported versions, use output-versions subcommand.
          
          [env: SQ_OUTPUT_VERSION=]

      --pep-cert-store <PATH>
          Specifies the location of a pEp certificate store.  sq does not use a pEp
          certificate store by default; it must be explicitly enabled using this
          argument or the corresponding environment variable, PEP_CERT_STORE.  The pEp
          Engine's default certificate store is at `$HOME/.pEp/keys.db`.
          
          [env: PEP_CERT_STORE=]

      --time <TIME>
          Sets the reference time as an ISO 8601 formatted timestamp.  Normally,
          commands use the current time as the reference time.  This argument allows
          the user to use a difference reference time.  For instance, when creating a
          key using `sq key generate`, the creation time is normally set to the
          current time, but can be overridden using this option.  Similarly, when
          verifying a message, the message is verified with respect to the current
          time.  This option allows the user to use a different time.
          
          TIME is interpreted as an ISO 8601 timestamp.  To set the certification time
          to July 21, 2013 at midnight UTC, you can do:
          
          $ sq --time 20130721 verify msg.pgp
          
          To include a time, say 5:50 AM, add a T, the time and optionally the
          timezone (the default timezone is UTC):
          
          $ sq --time 20130721T0550+0200 verify msg.pgp

      --trust-root <FINGERPRINT|KEYID>
          Considers the specified certificate to be a trust root. Trust roots are used
          by trust models, e.g., the Web of Trust, to authenticate certificates and
          User IDs.

  -v, --verbose
          Be more verbose.
```

A workaround for any of this is to re-define your globals in the subcommand but hidden.  Clap will still propagate the value for these options up to the parent command, behaving the same.  For an example of cargo taking advantage of this overriding of globals, see rust-lang/cargo#12959.

Considering the alternatives, the workarounds, and the niche nature of this (from my perspective), I would be hesitant to add a new toggle within clap.  Each toggle comes with a binary size, compile time, and API bloat cost and we try to weigh that against the value added.  In this case, I'm not seeing enough value.

---

_Comment by @nwalfield on 2024-02-16 13:47_

Thanks for the pointers.  I'll take a look and see if I can come up with something that works for us!

---

_Comment by @nwalfield on 2024-02-21 13:50_

[Here's what we ended up doing](https://gitlab.com/sequoia-pgp/sequoia-sq/-/merge_requests/112).

Basically, `cli` is a `Clap::Command`, where the global options are configured to be shown.  I now use `cli::try_get_matches` to figure out if the help output should be displayed.  If the help output should be displayed, I compare the help output (`err.render()`) to `cli.render_long_help()` and `cli.render_help()` to determine if the top-level help is being displayed.  If not so, I rewrite `cli` to hide the global options (`cli::build`).

If you have ideas on how to further improve this, I'd be happy to hear them!

```rust
    let mut cli = cli::build(true);
    let matches = cli.clone().try_get_matches();
    let c = match matches {
        Ok(matches) => {
            cli::SqCommand::from_arg_matches(&matches)?
        }
        Err(err) => {
            use clap::error::ErrorKind;
            if err.kind() == ErrorKind::DisplayHelp
                || err.kind() == ErrorKind::DisplayHelpOnMissingArgumentOrSubcommand
            {
                let output = err.render();
                let output = if output == cli.render_long_help() {
                    Some(cli::build(false).render_long_help())
                } else if output == cli.render_help() {
                    Some(cli::build(false).render_help())
                } else {
                    None
                };

                if let Some(output) = output {
                    if err.use_stderr() {
                        eprint!("{}", output);
                    } else {
                        print!("{}", output);
                    }
                    std::process::exit(err.exit_code());
                }
            }
            err.exit();
        }
    };
```

---

_Comment by @epage on 2024-02-21 15:15_

... that seems a lot more brittle than what I suggested earlier

> A workaround for any of this is to re-define your globals in the subcommand but hidden. Clap will still propagate the value for these options up to the parent command, behaving the same. For an example of cargo taking advantage of this overriding of globals, see https://github.com/rust-lang/cargo/pull/12959.

---

_Referenced in [moonbitlang/moon#281](../../moonbitlang/moon/pulls/281.md) on 2024-09-11 02:40_

---
