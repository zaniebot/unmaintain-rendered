---
number: 6016
title: "external_subcommand does not 'tab complete' in ZSH"
type: issue
state: open
author: Alpha1337k
labels:
  - C-enhancement
  - A-completion
  - S-waiting-on-design
assignees: []
created_at: 2025-05-26T18:04:13Z
updated_at: 2025-11-03T21:59:36Z
url: https://github.com/clap-rs/clap/issues/6016
synced_at: 2026-01-07T13:12:20-06:00
---

# external_subcommand does not 'tab complete' in ZSH

---

_Issue opened by @Alpha1337k on 2025-05-26 18:04_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.38

### Describe your use case

This problem exists in ZSH, and is seen in for example the `uv` tool:

```
uv run <TAB>

# no results, expected files and variables.
```

This is because of `external_subcommand`, used in this setting:

```
#[derive(Parser, Debug, Clone)]
pub enum ExternalCommand {
    #[command(external_subcommand)]
    Cmd (
        Vec<OsString>,
	),
}
```
[link](https://github.com/astral-sh/uv/blob/main/crates/uv-cli/src/lib.rs#L2686C1-L2690C2)

```
#[derive(Args)]
pub struct RunArgs {
...
    /// The command to run.
    ///
    /// If the path to a Python script (i.e., ending in `.py`), it will be
    /// executed with the Python interpreter.
    #[command(subcommand)]
    pub command: Option<ExternalCommand>,
...
}
```
[link](https://github.com/astral-sh/uv/blob/main/crates/uv-cli/src/lib.rs#L2890)

In the ZSH completion script there is no 'catch-all', only the options are specified.

I tried to recreate this issue, and I found that for bash and fish the files are suggested.


### Describe the solution you'd like

A catch all on the completions in case of external_subcommands would do the trick;

Locally I've tried the following:

```
clap_complete/src/aot/shells/zsh.rs:353
...
else if parent.is_allow_external_subcommands_set() {
	// If the command has an external subcommand value parser, we need to
	// add a catch-all for the subcommand. Otherwise there would be no autocompletion whatsoever.
	segments.push(String::from("\"*::external_command:_default\" \\"));
}
...
```
This adds `"*::external_command:_default" \ ` to the completions, and now the tab complete works.

I'm not sure if the change is 100% correct in all cases, therefore this is an issue, not an pr.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Alpha1337k on 2025-05-26 18:04_

---

_Label `A-completion` added by @epage on 2025-05-27 17:30_

---

_Label `S-waiting-on-design` added by @epage on 2025-05-27 17:30_

---

_Comment by @epage on 2025-05-27 17:32_

I only use bash and only a basic set of functionality.  I rarely enable completions and never author them.  I'm in a bad spot for evaluating a suggestion or implementing a fix.  If someone wants to propose something and provide the background for why it is the right call, I'd be willing to accept it.

We also need to figure out what we want to do about this for #3166.

---

_Referenced in [clap-rs/clap#6027](../../clap-rs/clap/pulls/6027.md) on 2025-06-09 13:55_

---

_Comment by @epage on 2025-11-03 21:59_

For native completions, the issue for this is #5653.

---

_Comment by @epage on 2025-11-03 21:59_

A related static completions issue is #3266.

---
