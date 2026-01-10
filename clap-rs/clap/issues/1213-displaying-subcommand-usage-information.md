---
number: 1213
title: Displaying subcommand usage information programmatically
type: issue
state: closed
author: rollandjb
labels: []
assignees: []
created_at: 2018-03-18T14:36:01Z
updated_at: 2018-08-02T03:30:20Z
url: https://github.com/clap-rs/clap/issues/1213
synced_at: 2026-01-10T01:26:45Z
---

# Displaying subcommand usage information programmatically

---

_Issue opened by @rollandjb on 2018-03-18 14:36_

Sorry if this is has been asked and answered (I did search), but I am new to Rust and Clap, and can't figure it out from the source..

I know I can programmatically print the usage information for a whole app with `app.print_help()`. How can I do the same thing for a subcommand. I would like to get the same result as entering the following command:

     my_app foo help

where `foo` is a subcommand that itself has subcommands.

I did discover `ArgMatches::usage()`, but that doesn't provide `FLAGS` and `SUBCOMMANDS` sections.

The reason I am looking for this is to print out the usage info when no subcommands are provided to a level that does have subcommands.

Thanks

---

_Comment by @kbknapp on 2018-03-19 18:06_

You can't currently do *exactly* that, however there is something that may be what you're asking for.

Try `AppSettings::ArgRequiredElseHelp`](https://docs.rs/clap/2.31.1/clap/enum.AppSettings.html#variant.ArgRequiredElseHelp) (which displays the help message if no arg or subcommand was used).

Also, if that doesn't solve it for you, could you give me some usage examples of what you're envisioning and I may be able to make a better suggestion.

---

_Label `T: RFC / question` added by @kbknapp on 2018-03-19 18:06_

---

_Comment by @rollandjb on 2018-03-20 07:41_

That's exactly what I want. Thank you.

---

_Closed by @rollandjb on 2018-03-20 07:41_

---
