---
number: 1284
title: Suggest PATTERN when passing an invalid flag as a pattern
type: issue
state: closed
author: flip111
labels:
  - C-bug
  - C-enhancement
assignees: []
created_at: 2018-06-03T12:33:39Z
updated_at: 2020-10-09T19:29:46Z
url: https://github.com/clap-rs/clap/issues/1284
synced_at: 2026-01-07T13:12:19-06:00
---

# Suggest PATTERN when passing an invalid flag as a pattern

---

_Issue opened by @flip111 on 2018-06-03 12:33_

### Rust Version

rustc 1.24.1

### Affected Version of clap

https://github.com/BurntSushi/ripgrep/blob/master/Cargo.lock#L47

### Suggestion

When passing a PATTERN which looks like a flag (starting with `--` or perhaps `-`) 
and when the parser expects a PATTERN
then suggest using `-- --myPattern`

Before error text
```
error: Found argument '--no-suggest' which wasn't expected, or isn't valid in this context
	Did you mean --no-sort-files?

USAGE:
    
    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f FILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list

For more information try --help
```

After help text (only in case when the parser accepts a PATTERN
```
error: Found argument '--no-suggest' which wasn't expected, or isn't valid in this context
	Did you mean --no-sort-files?
        If you tried to supply `--no-suggest` as a PATTERN use `rg -- --no-suggest`

USAGE:
    
    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f FILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list

For more information try --help
```

Addresses https://github.com/BurntSushi/ripgrep/issues/936

---

_Comment by @kbknapp on 2018-06-04 21:18_

This is what we do for subcommands, so adding it for args should be a quick fix!

Thanks for bringing it to my attention as I've been out for a few weeks.

---

_Label `T: bug` added by @kbknapp on 2018-06-04 21:19_

---

_Label `T: enhancement` added by @kbknapp on 2018-06-04 21:19_

---

_Label `P2: need to have` added by @kbknapp on 2018-06-04 21:19_

---

_Label `D: easy` added by @kbknapp on 2018-06-04 21:19_

---

_Label `W: 2.x` added by @kbknapp on 2018-06-04 21:19_

---

_Label `C: errors` added by @kbknapp on 2018-06-04 21:19_

---

_Comment by @rharriso on 2019-06-28 06:03_

I have a branch work on this:

https://github.com/rharriso/clap/tree/maybe-suggest-positional-arguments

---

_Referenced in [clap-rs/clap#1514](../../clap-rs/clap/pulls/1514.md) on 2019-06-30 23:56_

---

_Referenced in [clap-rs/clap#2154](../../clap-rs/clap/pulls/2154.md) on 2020-10-06 12:08_

---

_Closed by @bors[bot] on 2020-10-09 19:29_

---

_Referenced in [clap-rs/clap#2766](../../clap-rs/clap/issues/2766.md) on 2022-10-13 14:03_

---
