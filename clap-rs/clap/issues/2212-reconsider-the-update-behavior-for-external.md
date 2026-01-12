```yaml
number: 2212
title: Reconsider the update behavior for external commands and other values-taking fields
type: issue
state: closed
author: lu-zero
labels:
  - C-enhancement
  - A-derive
assignees: []
created_at: 2020-11-14T12:50:57Z
updated_at: 2021-08-13T19:51:22Z
url: https://github.com/clap-rs/clap/issues/2212
synced_at: 2026-01-12T16:14:12Z
```

# Reconsider the update behavior for external commands and other values-taking fields

---

_@lu-zero_

As discussed [here](https://github.com/clap-rs/clap/pull/1878#discussion_r523415453).

Updating a struct that has an external command field has it replaced completely, other options are to extend the Vec with the additional arguments if the command is the same. The behavior should stay consistent with the other places that take a list of values.


---

_Label `T: new feature` added by @lu-zero on 2020-11-14 12:50_

---

_Label `T: enhancement` added by @pksunkara on 2020-11-14 12:52_

---

_Label `T: new feature` removed by @pksunkara on 2020-11-14 12:52_

---

_Label `C: derive macros` added by @pksunkara on 2020-11-17 05:41_

---

_Comment by @pksunkara on 2021-08-13 00:57_

@epage Do you want to tackle this since you have fresh context regarding this code? (I know it's not 3.0)

---

_Comment by @epage on 2021-08-13 17:01_

Its unclear to me whether this is the "the right thing", at least to do unilaterally.  Merging is context-specific and we don't have the context to know what to do.  
- Does the external subcommand have "overrides self" setup for its fields or will it want a fresh set each time?
- Is a subcommand being used within that external subcommand?  We'd have to have that knowledge to know how to merge them, like our native subcommand merging logic.

---

_Comment by @pksunkara on 2021-08-13 17:02_

Assuming the field type is `Vec<OsString>`, is it currently completely replaced when doing update? If yes, then I would close this issue since the other ways have too many complications.

---

_Comment by @epage on 2021-08-13 17:20_

While we don't have a test case for it, I believe we completely replace:
```rust
                // Fallback to `from_arg_matches`
                Kind::ExternalSubcommand => None,
```

I can go add a test case real quick

---

_Comment by @pksunkara on 2021-08-13 17:21_

> I can go add a test case real quick

Let's do that and close this issue.

---

_Referenced in [clap-rs/clap#2684](../../clap-rs/clap/pulls/2684.md) on 2021-08-13 17:33_

---

_Closed by @pksunkara on 2021-08-13 19:51_

---

_Referenced in [clap-rs/clap#4974](../../clap-rs/clap/issues/4974.md) on 2023-06-19 17:18_

---
