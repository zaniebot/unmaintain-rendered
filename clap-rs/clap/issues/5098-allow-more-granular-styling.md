---
number: 5098
title: Allow more granular styling
type: issue
state: open
author: AndreasBackx
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2023-08-28T17:44:15Z
updated_at: 2023-08-28T18:46:41Z
url: https://github.com/clap-rs/clap/issues/5098
synced_at: 2026-01-10T01:28:07Z
---

# Allow more granular styling

---

_Issue opened by @AndreasBackx on 2023-08-28 17:44_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.4.1

### Describe your use case

This is a follow-up from https://github.com/clap-rs/clap/issues/3234#issuecomment-1562999659.

There are subcommands, or particular arguments sometimes that would benefit from having particular colouring to express what effect they could have. Imagine the fictional CLI `cluster-manager` which has a `delete` subcommand. It would be nice to reflect that this will actually delete a cluster. Perhaps that subcommand will have a prompt to confirm whether the user wants to delete a cluster, to allow for automation the `--yes` flag might be added. That `--yes` flag might also want to be coloured in a particular way to signify it's a dangerous flag to use.

### Describe the solution you'd like

The current behaviour is when a subcommand has a style, it overrides the subcommand's. The first change would be to allow the subcommand to change its styling. Then, similarly to how a command's styling works, tune it to allow for overriding a command's style for:
- A particular argument
- A particular option
- A particular subcommand

Unless I am missing anything, this should allow any generated line in `--help` to also be uniquely styled:


```
$ cli --help
Description.

USAGE:
    cli [OPTIONS] [-- <args>...]

ARGS:
	<args>... Options passed to whatever.

OPTIONS:
	--something
		Something very important.

COMMANDS:
	delete
		DANGEROUS BE AWARE!
```

We could style `<args>...`, `--something`, and `delete` uniquely.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @AndreasBackx on 2023-08-28 17:44_

---

_Label `A-help` added by @epage on 2023-08-28 18:10_

---

_Label `S-waiting-on-design` added by @epage on 2023-08-28 18:10_

---

_Comment by @epage on 2023-08-28 18:46_

This idea was inspired by https://github.com/click-contrib/click-help-colors

Unfortunately, not seeing a quick reference, so from what I gather they allow
- help_headers_color
  - You can set the color for all headings but not on a per-heading basis
  - Maybe we could change `help_heading` to accept a `StyledStr` and then we have a way to detect if styling is present and only apply ours if so
  - We might also want to wait on this to design it with help heading before / after text (we don't already have an issue for this?) in case it would be better handled another way
- help_options_color
  - We support this today
- help_options_custom_colors (maps arg or command to color)
  - We could add `Arg::styles` and the help / usage could read that though it might require some more book keeping
  - This might make sorting of arguments more complex and we'll need to take that into account
- prog_name_color, version_color, message_color (for `--version`)
  - We allow people to pass in a colored version already which I think covers this
- All of that can be set on a per-command basis
  - We only support it on an application basis
  - Part of this is an architectural issue as we are using our plugin system and it doesn't know how to merge when propogating

In addition,. this Issue calls out styling of subcommand names.  The challenge here is that we don't want names to be accepted as styled because we do string matching on them.  API wise, this might also relate to #1553

---
