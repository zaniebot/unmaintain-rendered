```yaml
number: 3940
title: "`args_conflicts_with_subcommands` does not overriding requireds on arguments"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - M-breaking-change
  - A-parsing
  - E-easy
assignees: []
created_at: 2022-07-16T02:17:12Z
updated_at: 2022-07-22T21:43:41Z
url: https://github.com/clap-rs/clap/issues/3940
synced_at: 2026-01-12T16:14:15Z
```

# `args_conflicts_with_subcommands` does not overriding requireds on arguments

---

_@epage_

Maintainer's notes

Normally, conflicts are two way and they override `required(true)`.  We aren't doing that with `args_conflicts_with_subcommands` while it can be worked around with `subcommand_negates_reqs`.  The main question is whether to consider this a breaking change or not.

--

### Discussed in https://github.com/clap-rs/clap/discussions/3892

<div type='discussions-op-text'>

<sup>Originally posted by **sasial-dev** July  1, 2022</sup>
How would I **not** have `-f` & `-p` in the subcommands?

```rust
    let cli = command!("edit-place")
        .propagate_version(true)
        .arg_required_else_help(true)
        .subcommand(
            command!("config")
                .about("Edit the favourites config")
                .subcommand_required(true)
                .subcommand(command!("add").about("Add a favourite"))
                .subcommand(command!("list").about("List all favourites"))
                .subcommand(command!("remove").about("Remove a favourite")),
        )
        .arg(arg!(-p --place <"place id"> "Place ID to open").global(false))
        .arg(arg!(-f --favourite <"favourite name"> "Favourite place to open").global(false))
        .group(
            ArgGroup::new("id")
                .required(true)
                .args(&["place", "favourite"]),
        )
        .get_matches();
```</div>

---

_Label `C-bug` added by @epage on 2022-07-16 02:17_

---

_Label `M-breaking-change` added by @epage on 2022-07-16 02:17_

---

_Label `A-parsing` added by @epage on 2022-07-16 02:17_

---

_Label `E-easy` added by @epage on 2022-07-16 02:17_

---

_Added to milestone `4.0` by @epage on 2022-07-16 02:17_

---

_Referenced in [clap-rs/clap#3974](../../clap-rs/clap/pulls/3974.md) on 2022-07-22 19:11_

---

_Closed by @epage on 2022-07-22 19:25_

---

_Comment by @sasial-dev on 2022-07-22 21:43_

Thank you!

---

_Referenced in [clap-rs/clap#5358](../../clap-rs/clap/issues/5358.md) on 2024-02-19 19:13_

---
