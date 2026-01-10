---
number: 5960
title: Extra newline printed when a flat subcommand has no arguments
type: issue
state: closed
author: BrandonXLF
labels: []
assignees: []
created_at: 2025-03-27T00:15:28Z
updated_at: 2025-03-27T01:40:21Z
url: https://github.com/clap-rs/clap/issues/5960
synced_at: 2026-01-10T01:28:19Z
---

# Extra newline printed when a flat subcommand has no arguments

---

_Issue opened by @BrandonXLF on 2025-03-27 00:15_

When `flatten_help` is enabled and a subcommand doesn't have any arguments, an extra newline is printed like below:

```
Usage: cmd
       cmd foo [OPTIONS] [TO_VER]
       cmd bar

cmd foo:
Run foo
  -f, --from <FROM>     The version to upgrade from
  [TO_VER]           The version to upgrade to

cmd bar:
Run bar


After help:
...
```

It should instead be:

```
Usage: cmd
       cmd foo [OPTIONS] [TO_VER]
       cmd bar

cmd foo:
Run foo
  -f, --from <FROM>     The version to upgrade from
  [TO_VER]           The version to upgrade to

cmd bar:
Run bar

After help:
...
```

---

_Referenced in [clap-rs/clap#5961](../../clap-rs/clap/pulls/5961.md) on 2025-03-27 00:16_

---

_Closed by @epage on 2025-03-27 01:40_

---
