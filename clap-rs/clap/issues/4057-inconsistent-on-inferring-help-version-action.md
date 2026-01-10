---
number: 4057
title: Inconsistent on inferring Help/Version action
type: issue
state: closed
author: epage
labels:
  - C-bug
  - S-waiting-on-decision
  - A-builder
  - A-derive
assignees: []
created_at: 2022-08-11T00:56:00Z
updated_at: 2022-08-11T02:48:08Z
url: https://github.com/clap-rs/clap/issues/4057
synced_at: 2026-01-10T01:27:50Z
---

# Inconsistent on inferring Help/Version action

---

_Issue opened by @epage on 2022-08-11 00:56_

As of at least #4056 in master, 
- `arg!(--help)` will implicitly get `ArgAction::Help`
- `Arg::new("help").long("help")` will implicitly get `ArgAction::Help`
- `#[clap(long)] help: bool` will implicitly get `ArgAction::SetTrue`

Questions
- Should these all be consistent?
- Which direction should these all go?

Considerations
- Work for creating a help/version flag after #4056 
- Ability to document default action behavior

---

_Label `C-bug` added by @epage on 2022-08-11 00:56_

---

_Label `S-waiting-on-decision` added by @epage on 2022-08-11 00:56_

---

_Label `A-builder` added by @epage on 2022-08-11 00:56_

---

_Label `A-derive` added by @epage on 2022-08-11 00:56_

---

_Added to milestone `4.0` by @epage on 2022-08-11 00:56_

---

_Comment by @epage on 2022-08-11 00:57_

For derive, I played with a field type of `()` with `help`/`version` ids causing it to infer the action.

Overall, I'm concerned with the magic / documentation for any exceptions to inferring like this.  I'm tempted to create tests to verify the workflow and to ensure that the experience isn't terrible and to remove all special casing.

---

_Referenced in [clap-rs/clap#4058](../../clap-rs/clap/pulls/4058.md) on 2022-08-11 01:53_

---

_Referenced in [clap-rs/clap#4059](../../clap-rs/clap/pulls/4059.md) on 2022-08-11 02:16_

---

_Closed by @epage on 2022-08-11 02:48_

---
