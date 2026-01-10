---
number: 5864
title: Long help only
type: issue
state: closed
author: nic-hartley
labels:
  - C-enhancement
assignees: []
created_at: 2025-01-01T23:55:30Z
updated_at: 2025-06-10T15:31:58Z
url: https://github.com/clap-rs/clap/issues/5864
synced_at: 2026-01-10T01:28:18Z
---

# Long help only

---

_Issue opened by @nic-hartley on 2025-01-01 23:55_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.23

### Describe your use case

I always want the long help to be displayed in every scenario, without having to manually write glue code for every single command and subcommand and edge case.

### Describe the solution you'd like

`long_help_only` in the same place as `disable_help_flag`, which makes the long help show up whenever either long or short help is requested.

### Alternatives, if applicable

1. Status quo: There is no way to do this without manually writing significant amounts of glue code to add a `--help` flag to every subcommand with the `HelpLong` action, *as well as* extra handling around parsing at every level (which I haven't even figured out! It's possible this isn't even possible!) to display the long help in case of usage errors/missing required subcommands, instead of the short help that's the default.
2. Delete short help entirely: This would make me personally happy because I've had to fight it every time I use clap, but I suspect I'm in the minority with that opinion, so I don't think it'd be a popular move.
3. A more general, command-wide formatting system. I think this is something clap would benefit from -- something I've unsuccessfully fought in the past is clap's default formatting, which I generally dislike (I don't like underlines in my CLI at all!) -- but it's a massively overengineered solution to this small and (I think) relatively simple use-case.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @nic-hartley on 2025-01-01 23:55_

---

_Comment by @epage on 2025-01-02 14:42_

>  long_help_only in the same place as disable_help_flag, which makes the long help show up whenever either long or short help is requested.

For each entry point we add like this, we make clap
- Harder to use because it makes it more difficult to find functionality people are looking for
- Slower to compiler
- Have a larger binary size

We are instead trying to focus on a common set of behaviors across applications and providing general extension points.

> Status quo: There is no way to do this without manually writing significant amounts of glue code to add a --help flag to every subcommand with the HelpLong action, as well as extra handling around parsing at every level (which I haven't even figured out! It's possible this isn't even possible!) to display the long help in case of usage errors/missing required subcommands, instead of the short help that's the default.

Why do you need "extra handling of parsing at every level"?

I feel I'm missing something in terms of why this is a lot of glue code.  If you use a derive, you can define one struct that you flatten into every subcommand struct.  If you use the builder API, its a single function to do the same.  Each one just needs to set one `Command` attribute and add a new `Arg`.

> A more general, command-wide formatting system. I think this is something clap would benefit from -- something I've unsuccessfully fought in the past is clap's default formatting, which I generally dislike (I don't like underlines in my CLI at all!) -- but it's a massively overengineered solution to this small and (I think) relatively simple use-case.

As I said, our focus is on general extension points.  We are tracking this overall at #3476.  I'd like to move help generation to be a behind a trait and allow people to select the help generation policy they want.  This will allow people to pull in what they need, reducing binary size and build times, and allow complete customization.  For example, we can split our current help generator into ShortHelpWriter, LongHelpWriter, TemplatedHelpWriter, StaticHelpWriter.  We could provide a composite type for any dynamic capabilities we want.

I don't see us writing a bespoke formatting system though. We have considered separating out the extracting of data used for generating help from the formatter, making it easier to write a custom formatter.

> which I generally dislike (I don't like underlines in my CLI at all!)

Are you aware of [`Command::styles`](https://docs.rs/clap/latest/clap/struct.Command.html#method.styles)?

---

_Comment by @epage on 2025-06-10 15:31_

As this is just a convenience and we have other plans being tracked to help with this, I'm going to go ahead and close this.  If there is a reason we should keep this open, let us know!

---

_Closed by @epage on 2025-06-10 15:31_

---
