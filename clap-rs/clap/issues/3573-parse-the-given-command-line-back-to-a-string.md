```yaml
number: 3573
title: Parse the given command-line back to a string?
type: issue
state: closed
author: picobyte
labels: []
assignees: []
created_at: 2022-03-24T13:50:35Z
updated_at: 2022-03-24T14:32:12Z
url: https://github.com/clap-rs/clap/issues/3573
synced_at: 2026-01-12T16:14:15Z
```

# Parse the given command-line back to a string?

---

_@picobyte_

For reproducibility it's common to keep track of command-line invocations in a file header (sequence alignments). Clap seems to miss a way to display the command-line, or at least the documentation is hard to find. Possibly also a search for 'command-line' or alias gives too much side noise.

The tool is developed here: https://github.com/NKI-GCF/rumidup
The changes are not yet in the production branch, though.

Using clap 3.1.0 with features "derive" and "cargo", I get the command struct using the command!() macro in src/io.rs, but I cannot find a straightforward get_command_line() function, or way to list parameters and arguments as invoked. Is there a way?

---

_Comment by @picobyte on 2022-03-24 14:17_

Nevermind, solved it using:
```rust
let args: Vec<String> = std::env::args().collect();
args.join(" ")
```

---

_Closed by @picobyte on 2022-03-24 14:17_

---

_Comment by @epage on 2022-03-24 14:32_

Technically, you probably want `shlex::join` rather than `.join(" ")`.

---
