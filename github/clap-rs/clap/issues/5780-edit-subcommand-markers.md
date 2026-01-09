---
number: 5780
title: Edit subcommand markers
type: issue
state: closed
author: mondeja
labels:
  - C-enhancement
  - A-help
assignees: []
created_at: 2024-10-17T20:27:21Z
updated_at: 2024-10-18T02:40:16Z
url: https://github.com/clap-rs/clap/issues/5780
synced_at: 2026-01-07T13:12:20-06:00
---

# Edit subcommand markers

---

_Issue opened by @mondeja on 2024-10-17 20:27_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.20

### Describe your use case

Currently, subcommand markers are generated as `<>` for required subcommands or `[]` for optional ones.

I would like to edit them. This would make transitions from other tools or rewritings much more easy.

For example, in Python by default are presented as `{cmd1,cmd2...}`, see https://docs.python.org/es/3/library/argparse.html#argparse.ArgumentParser.add_subparsers at:

> metavar - string presenting available subcommands in help; by default it is `None` and presents subcommands in form {cmd1, cmd2, ..}

### Describe the solution you'd like

I would like a to call a method like `subcommand_markers("{", "}")`. For example:

```rust
Command::new("foo")
    .subcommand_value_name("run,install")
    .subcommand_markers("{", "}")
```

Running `foo --help`:

```
Usage: foo {run,install}

...
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @mondeja on 2024-10-17 20:27_

---

_Label `A-help` added by @epage on 2024-10-18 02:36_

---

_Comment by @epage on 2024-10-18 02:40_

In clap, we try to balance a couple of factors
- Encourage people towards best / common practices while this goes off into less common practices
- Be careful of our built-in configurability because extending the API comes at the cost of build-times / binary size and the larger the API is, the harder it is for users to find features, making new features reduce the value of every feature of clap

We  are interested in decoupling formatting and maybe that would be a way for you to provide a custom skin to handle these different formats.  We are tracking this in #2914 and closing in favor of that.

---

_Closed by @epage on 2024-10-18 02:40_

---
