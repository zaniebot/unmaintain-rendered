---
number: 5354
title: Extend help subcommand
type: issue
state: open
author: nwalfield
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-help
assignees: []
created_at: 2024-02-16T09:45:00Z
updated_at: 2024-02-17T09:20:08Z
url: https://github.com/clap-rs/clap/issues/5354
synced_at: 2026-01-07T13:12:20-06:00
---

# Extend help subcommand

---

_Issue opened by @nwalfield on 2024-02-16 09:45_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.0

### Describe your use case

[`sq`](https://gitlab.com/sequoia-pgp/sequoia-sq) is the command-line front end for [`sequoia-openpgp`](https://gitlab.com/sequoia-pgp/sequoia).  It has a dozen subcommands, and most of the subcommands have their own subcommands.  This deep hierarchical structure means getting an overview of `sq`'s functionality is not straightforward.


### Describe the solution you'd like

Most non-fiction books include a table of contents, which gives the reader a quick overview of what is covered and where.  It would be nice if clap could offer similar functionality.

One idea what we had is to extended the `help` subcommand with an option, perhaps `-a`, which recursively lists all subcommands.  Right now, `sq help` shows:

```
$ sq help -a
...
Commands:
  encrypt    Encrypts a message
  decrypt    Decrypts a message
  sign       Signs messages or data files
  verify     Verifies signed messages or detached signatures
  inspect    Inspects data, like file(1)
  cert       Manages certificates
  key        Manages keys
  pki        Authenticate certs using the Web of Trust
  autocrypt  Communicates certificates using Autocrypt
  network    Retrieves and publishes certificates over the network
  toolbox    Tools for developers, maintainers, and forensic specialists
  version    Detailed version and output version information
...
```

`-a` could perhaps show something like:

```
$ sq help -a
...
Commands:
  encrypt    Encrypts a message
  decrypt    Decrypts a message
  sign       Signs messages or data files
  verify     Verifies signed messages or detached signatures
  inspect    Inspects data, like file(1)
  cert       Manages certificates
  cert import  Imports certificates into the local certificate store
  cert export  Exports certificates from the local certificate store
  cert lint    Checks certificates for issues
  key        Manages keys
  key generate               Generates a new key
  key password               Changes password protecting secrets
  key expire                 Changes expiration times
  key revoke                 Revoke a certificate
  key userid                 Manages User IDs
  key subkey                 Manages Subkeys
  key extract-cert           Converts a key to a cert
  key attest-certifications  Attests to third-party certifications
  key adopt                  Binds keys from one certificate to another
  pki        Authenticate certs using the Web of Trust
  pki authenticate  Authenticate a binding
  pki lookup        Lookup the certificates associated with a User ID
  pki identify      Identify a certificate
  pki certify       Certifies a User ID for a Certificate
  pki link          Manages authenticated certificate and User ID links
  pki list          List all authenticated bindings (User ID and certificate pairs)
  pki path          Verify the specified path
  ...
```

I've spent some time looking at clap's documentation, but I couldn't find a way to extend the automatically generated `help` subcommand.  One option would be to disable the `help` subcommand and reimplement it in `sq` with the added functionality.  This seems brittle, however.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @nwalfield on 2024-02-16 09:45_

---

_Comment by @epage on 2024-02-16 12:20_

I know at least `git` has the `help -a` flag (along with `help -g`, see #5332).  Cargo has the top-level `--list`.  Know of any other prior art?

My first concern is with how people discover this.  People aren't likely to run `sq help help`.  Git advertises the flag with an `after_help`
```
'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
See 'git help git' for an overview of the system.
```
I would be loathe to add a blanket after help for people.  Maybe them adding it is their way to "opt in".

My second concern is I'm unsure if a flag is the right approach.  I feel like this would be better served as a "topic" (`sq help commands`) rather than a flag.  Mostly, this feels more consistent to me.  In part, I've wondered about allowing you to take arbitrary commands and inserting a `help` in the middle (much like you can with `--help`) which means we need to ignore all flags.

---

_Label `S-waiting-on-decision` added by @epage on 2024-02-16 12:21_

---

_Label `A-help` added by @epage on 2024-02-16 12:21_

---

_Referenced in [rust-lang/cargo#12114](../../rust-lang/cargo/issues/12114.md) on 2024-02-16 13:19_

---

_Referenced in [clap-rs/clap#5355](../../clap-rs/clap/issues/5355.md) on 2024-02-16 13:27_

---

_Comment by @nwalfield on 2024-02-16 13:52_

> I know at least git has the help -a flag (along with help -g, see https://github.com/clap-rs/clap/discussions/5332). Cargo has the top-level --list. Know of any other prior art?

`gpg` has `--dump-options`.

> I would be loathe to add a blanket after help for people. Maybe them adding it is their way to "opt in".

I don't completely understand the suggestion.  Do you mean people adding a "table-of-contents" flag is opting in, or people adding an after_help is opting in?  Isn't the latter ambiguous, as someone could add an after_help for other reasons?

> My second concern is I'm unsure if a flag is the right approach. I feel like this would be better served as a "topic" (sq help commands) rather than a flag.

I'd be fine with this.  `-a` was just inspired by `git`.

---

_Comment by @epage on 2024-02-16 14:56_

> I don't completely understand the suggestion. Do you mean people adding a "table-of-contents" flag is opting in, or people adding an after_help is opting in? Isn't the latter ambiguous, as someone could add an after_help for other reasons?

One idea is that we support `-a` unconditionally but leave it to the caller to decide if they want to "support it" by them specifying the `after_help`, rather than us doing it automatically.

---

_Comment by @nwalfield on 2024-02-17 09:20_

Thanks for the clarification.  I think you are saying that `-a` is always implemented, but it is up to the application to document that.  I think that is okay.  But, it should probably still be documented under `help`, even if most people probably do not run `cli help help`:

```
$ sq help help
Print this message or the help of the given subcommand(s)

Usage: sq help [COMMAND]...

Arguments:
  [COMMAND]...  Print help for the subcommand(s)
```

---
