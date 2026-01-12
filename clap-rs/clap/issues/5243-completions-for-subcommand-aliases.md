```yaml
number: 5243
title: completions for subcommand aliases
type: issue
state: closed
author: dfgordon
labels:
  - C-enhancement
  - E-hard
  - A-completion
assignees: []
created_at: 2023-12-03T17:27:13Z
updated_at: 2024-03-21T14:38:17Z
url: https://github.com/clap-rs/clap/issues/5243
synced_at: 2026-01-12T16:14:16Z
```

# completions for subcommand aliases

---

_@dfgordon_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.2.7

### Describe your use case

A subcommand called `catalog` has aliases `dir` and `ls`.  When `clap_complete` generates the zsh script it omits the aliases.  As a result if a user tries `ls` they will not get the completions, and even worse, it tends to block the shell's natural path completions, making the tool less friendly instead of more.

### Describe the solution you'd like

This appears easy to solve, you simply copy the block of the subcommand substituting the alias, such as
```
(mycommand)
...stuff
;;
(mycom)
...same stuff
;;
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @dfgordon on 2023-12-03 17:27_

---

_Label `E-hard` added by @epage on 2023-12-04 17:37_

---

_Label `A-completion` added by @epage on 2023-12-04 17:37_

---

_Comment by @dfgordon on 2023-12-10 16:11_

Without intervention zsh offers the aliases.  With the intervention proposed above (tested) it still does so but also allows arguments to be completed.  So far I didn't find any downside.

I have not tried with the other shells, except to note that bash is already handling it without any intervention.

![Screenshot 2023-12-10 at 11 08 20 AM](https://github.com/clap-rs/clap/assets/13408285/7c1261db-bee2-4913-b0ab-9a83db8bd625)

---

_Comment by @dfgordon on 2023-12-10 16:27_

Minor note, tried all with clap and clap_complete 4.4.4, all results the same.

---

_Comment by @nasso on 2024-03-21 14:33_

isn't this a duplicate of #4265 ?

---

_Comment by @epage on 2024-03-21 14:38_

Thanks for catching that!

---

_Closed by @epage on 2024-03-21 14:38_

---
