---
number: 5504
title: "Provide an attribute that implements a common `--completions <SHELL>` convention without boilerplate"
type: issue
state: closed
author: lgarron
labels:
  - C-enhancement
  - A-completion
assignees: []
created_at: 2024-05-24T05:54:11Z
updated_at: 2024-08-10T00:49:48Z
url: https://github.com/clap-rs/clap/issues/5504
synced_at: 2026-01-07T13:12:20-06:00
---

# Provide an attribute that implements a common `--completions <SHELL>` convention without boilerplate

---

_Issue opened by @lgarron on 2024-05-24 05:54_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

N/A

### Describe your use case

A lot of programs now implement one of the following:

- Flag: `program --completions <SHELL>`
- Command: `program completions <SHELL>`

I *loooove* `clap` and `clap_complete`, and would love for this to be so simple to configure that I can configure it for a project (or contribute it to another project using `clap`) in minimal code.

While it would be nice to support both the flag and command version, I think command version is more fraught[^1], so I will focus on a proposal for the flag version.

[^1]: If you want to figure out whether a program supports something, I think a flag is generally much safer to try than a command. For example, `rm --completions fish` is basically safe to run, while `rm completions fish` is a recipe for a bad time.

### Describe the solution you'd like

I'd love to get full completions functionality with syntax like this:

```rust
use clap_complete::{Shell};

// …
struct ProgramArgs {
    #[clap(completions)]
    completions: Option<Shell>,
}
```

- `clap_complete` supports a `completions` attribute.
  - This attribute must be defined on a flag named `completions` at the top level of the program. (Or I think this should at least generate  warning unless explicitly silenced.)
  - It must have the type `Option<Shell>`.
- `clap_complete` automatically generates default documentation for the flag, something like this:

```
Print completions for the given shell. When this flag is passed, any other input is ignored and the program will terminate immediately after printing the completions.

Completions can be loaded and stored permanently by package managers, or they can be loaded into a given shell as follows:

source <(<COMMAND_NAME> --completions bash) # bash
source <(<COMMAND_NAME> --completions zsh) # zsh
<COMMAND_NAME> --completions fish | source # fish
… # …
```

- When the flag is passed, `clap_complete` ignores any other arguments/input, prints completions, and exits with code `0` ([example](https://github.com/lgarron/folderify/blob/d3a603f005c4db67ff05c7ab3e5767cdf4380728/src/options.rs#L162-L176)).
- I haven't completely thought this through, but:
  - If the flag is passed without a shell argument, the program could give some sort of specific signal that it supports the convention described here. Maybe a particular exit code? Maybe a hardcoded string as part of `stdout` or `stderr` that can be `grep`ped with low false positives?
  - The main program argument struct could take a `default_completions_behaviour` attribute that automatically injects the flag?

### Alternatives, if applicable

The main alternative is to leave this for each program to implement individually.

However, I think this is not ideal in the long term:

- It requires someone using `clap` to do some research and manually figure out how to integrate this into their code. By contrast, a single `clap_complete` directive is trivial to try out, and will work perfectly the first time in a lot of cases. Now there's a value proposition!
- Individual implementations can diverge in behaviour, or have bugs. In particular, if it's up to the programmer to print the completions manually, they may not do so immediately upon parsing the commandline arguments. This could lead to subtle interactions between flags, and unwanted side effects.
- An implementation will automatically support (and document support for) all shells by default.
- A default implementation in `clap_complete` will automatically stay up to date over time with upgrades to `clap_complete`. No need to keep the flag's documentation up to date manually for each program if there are feature changes (such as newly supported shells).
  - Another example is shell compatibility fixes. For example, Rust currently panics when I try to run `source <(folderify --completions bash)` in `bash` on my computer, because of an issue with piping. If I find a fix, I'd love to contribute that to `clap_complete` and upgrade dependencies instead of fixing a copy of the code in each of my programs. Since most shells are still actively getting new features and shell syntax/semantics can be really subtle, I'd bet there will be more such issues over time, and it would be much more powerful to have all the compatibility handled in one place.

I'd also love for this to serve as the basis for a standard convention for discoverable completions that can be used by package managers and shells. Hence my emphasis on consistent behaviour with consistent invocation syntax.

For example, Homebrew makes it a one-liner to support completions for a given program, but you don't get them for a given program if someone didn't add that line manually.[^2] I can imagine a package manager that automatically invokes `--completions` and loads them automatically, or at least introduces a linter for package definitions that highlights when completions seem to be available (but are not explicitly marked as supported/unsupported). This kind of functionality could also integrate with something like `cargo install` or shells directly.


[^2]: In fact, I was motivated to file this issue after I found that Homebrew was missing completions for `tailscale`. They were super quick to accept [my PR to add support](https://github.com/Homebrew/homebrew-core/pull/172679). But that means everyone is only getting these completions going forward, when they were actually already available!

### Additional Context

I know I'm probably not thinking of some details (e.g. how to avoid pulling in the standard library, how to have a program signal support for this convention without complications for legacy programs), but I think at an implementation level this should be a fairly moderate amount of code? But I know I'm asking for kind of bold stance on paving the path for a kind of hardcoded convention, when `clap` and `clap_complete` are all about versatility and flexibility in general.

I'd be glad to contribute a PR if maintainers are on board and can give me a few pointers.

---

_Label `C-enhancement` added by @lgarron on 2024-05-24 05:54_

---

_Renamed from "Provide a no-boilerplate "conventional mode"" to "Provide a `completions` attribute that implements a common convention without boilerplate" by @lgarron on 2024-05-24 05:54_

---

_Renamed from "Provide a `completions` attribute that implements a common convention without boilerplate" to "Provide an attribute that implements a common `--completions <SHELL>` convention without boilerplate" by @lgarron on 2024-05-24 06:15_

---

_Label `A-completion` added by @epage on 2024-05-25 16:27_

---

_Comment by @epage on 2024-05-25 16:28_

We are offering this with our newer, in-work completion system.  See [CompleteCommand](https://docs.rs/clap_complete/latest/clap_complete/dynamic/shells/enum.CompleteCommand.html).

---

_Comment by @lgarron on 2024-05-25 22:17_

Oh, that is fabulous to see! 🤩

---

_Comment by @epage on 2024-08-10 00:49_

As I believe this is resolved with the native completions, I'm going to close this.  If this was incorrect, let us know!

You can track stabilization at #3166

---

_Closed by @epage on 2024-08-10 00:49_

---
