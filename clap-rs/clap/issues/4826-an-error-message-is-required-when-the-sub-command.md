```yaml
number: 4826
title: An error message is required when the sub-command is not provided.
type: issue
state: closed
author: heisen-li
labels:
  - C-enhancement
assignees: []
created_at: 2023-04-05T10:39:07Z
updated_at: 2023-04-19T02:31:45Z
url: https://github.com/clap-rs/clap/issues/4826
synced_at: 2026-01-12T16:14:16Z
```

# An error message is required when the sub-command is not provided.

---

_@heisen-li_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.2.1

### Describe your use case

When method [subcommand](https://github.com/clap-rs/clap/blob/master/clap_builder/src/parser/matches/arg_matches.rs#L904) is used, if there is no subcommand in the input, a function should be provided to return the prompt information.


### Describe the solution you'd like

Stops the current operation, displays the help information, and exits.

For example:
```
let (sc, args) = args.subcommand().or_error("Error: Sorry, you did not provide the subcommand, Run the `xxx yyy -h` command to obtain more help information.");
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @heisen-li on 2023-04-05 10:39_

---

_Comment by @epage on 2023-04-05 13:03_

Are you looking for [`Command::subcommand_required`](https://docs.rs/clap/latest/clap/struct.Command.html#method.subcommand_required)?

---

_Comment by @heisen-li on 2023-04-17 12:30_

The function is similar to this function, but it needs to be implemented for the structure [ArgMatches](https://docs.rs/clap_builder/4.2.2/src/clap_builder/parser/matches/arg_matches.rs.html#67). It is required that there must be a subcommand in the instance, if not, an error or prompt is returned.

---

_Comment by @epage on 2023-04-17 18:34_

For the Issue as written, the solution is `Command::subcommand_required` with `ArgMatches.subcommand().expect("always present due to `subcommand_required`")`

If you would like to prompt the user, why not use `unwrap_or_else`?

---

_Comment by @heisen-li on 2023-04-18 02:49_

![image](https://user-images.githubusercontent.com/46313511/232657188-98d13242-1357-4d86-b260-b29af0d16547.png)

I know there are other ways to do it.

I try to understand what you mean from here, doesn't this problem need to be solved in clap?

---

_Comment by @epage on 2023-04-18 16:33_

If we weren't concerned about that being a breaking change in cargo, this would be resolved by my earlier comment:

> For the Issue as written, the solution is `Command::subcommand_required` with `ArgMatches.subcommand().expect("always present due to `subcommand_required`")`

No new features are needed from clap to do this.

This is what I was referring to in that comment.

---

_Comment by @heisen-li on 2023-04-19 02:31_

Sorry for misunderstanding you. My English is not good, so I rely on Google Translate. This issue will be closed. thank you for your reply.

---

_Closed by @heisen-li on 2023-04-19 02:31_

---
