```yaml
number: 3048
title: ArgMatches method to get the alias used (and long or short version)
type: issue
state: open
author: MarcelRobitaille
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-parsing
assignees: []
created_at: 2021-11-24T18:25:35Z
updated_at: 2025-04-08T21:48:27Z
url: https://github.com/clap-rs/clap/issues/3048
synced_at: 2026-01-12T16:14:14Z
```

# ArgMatches method to get the alias used (and long or short version)

---

_@MarcelRobitaille_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

2.33.3

### Describe your use case

I am trying to determine which variant of an option was used. For example, did the user type `--number` or just `-n`.

### Describe the solution you'd like

I think `ArgMatches` should have a method similar to `value_of` that would return this.

```rust
let matches = App::new("cli")
    .arg(
        Arg::with_name("number")
            .short("n")
            .long("number")
            .alias("limit")
            .takes_value(true),
    )
    .get_matches_from(vec!["cli", "--limit", "10"]);

assert_eq!(matches.variant_of("number").unwrap(), "--limit");

```

### Alternatives, if applicable

Here is my workaround. 

```rust
use std::env;

let matches = App::new("cli")
    .arg(
        Arg::with_name("number")
            .short("n")
            .long("number")
            .alias("limit")
            .takes_value(true),
    )
    .get_matches();

assert_eq!(
    env::args()
        .nth(matches.index_of("number").unwrap() - 1)
        .unwrap(),
    "--limit"
);
```

### Additional Context

I am trying to print more helpful error messages. For example, instead of `Failed to parse argument "number"` I could print `Failed to parse argument "--limit"`.

---

_Label `T: new feature` added by @MarcelRobitaille on 2021-11-24 18:25_

---

_Referenced in [epage/clapng#244](../../epage/clapng/issues/244.md) on 2021-12-06 23:13_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:07_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:07_

---

_Label `A-parsing` added by @epage on 2021-12-13 22:32_

---

_Label `S-waiting-on-decision` added by @epage on 2021-12-13 22:32_

---

_Referenced in [clap-rs/clap#5748](../../clap-rs/clap/issues/5748.md) on 2024-09-24 21:40_

---

_Comment by @lewisboon on 2025-04-08 21:48_

There's a usecase for this in [uutils/coreutils](https://github.com/uutils/coreutils/blob/main/src/uu/df/src/df.rs#L132).

To match the functionality of the "standard" `df` program, they want to adjust the error text to show the flag that was used.

---
