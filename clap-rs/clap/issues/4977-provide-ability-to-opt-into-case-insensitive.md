```yaml
number: 4977
title: "Provide ability to 'opt into' case insensitive option/argument names"
type: issue
state: open
author: jerrywrice
labels:
  - C-enhancement
assignees: []
created_at: 2023-06-19T21:54:47Z
updated_at: 2023-06-20T14:29:41Z
url: https://github.com/clap-rs/clap/issues/4977
synced_at: 2026-01-12T16:14:16Z
```

# Provide ability to 'opt into' case insensitive option/argument names

---

_@jerrywrice_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.6

### Describe your use case

It would be beneficial for a new version of clap to provide a means which permits any command line argument name (for example '--PORT=dev1') to be matched in a case insensitive manner. Currently the run-time parser displays an error message indicating it found an unexpected argument (whereas a case-insensitive compare would in-fact match). The idea that fifty years ago the Unix folks chose that command line argument names should be case sensitive is irrelevant, since newly developed applications would need to 'opt into' this as it wouldn't be the default.

### Describe the solution you'd like

Adding a new setter method **'ignore_name_case(bool)'** to clap::builder::Command' seems fine.

.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @jerrywrice on 2023-06-19 21:54_

---

_Comment by @epage on 2023-06-20 01:31_

What isn't clear from this is "why".

What value is this providing end-users?  Under what situations is someone typing `--PORT` rather than `--port`?  For example, in #4955 someone requested case insensitivity but the actual relevant user patterns for them were finite and they were better served by explicitly listing the situations with aliases.

I'd also recommend making a case by looking for prior art.  Are there well received CLIs that accept case insensitive flags and subcommands?  Are there major parsers that support it?

---

_Comment by @jerrywrice on 2023-06-20 05:09_

'Why' ... so that a user of an application with utilizes the 'clap' crate isn't forced to edit and reenter their application command line due to mistyping an command line argument (switch) name which differs only in its character case. Note that Windows application often have command line arguments which allow case-insensivity with the switch prefixes (names). Check out the 'Dir' command for instance (and its options). 

---

_Comment by @epage on 2023-06-20 14:29_

For straight typos on a US standard keyboard, shift is likely to be accidentally be hit when typing a flag or a command name.

For people assuming the wrong case, with the convention being for lower case, docs being in lower case, and (optional completions) helping towards lower case, it is unclear to me how someone would run into this.

---
