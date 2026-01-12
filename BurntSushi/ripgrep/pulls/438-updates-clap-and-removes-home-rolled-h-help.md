```yaml
number: 438
title: updates clap and removes home rolled -h/--help distinction
type: pull_request
state: merged
author: kbknapp
labels: []
assignees: []
merged: true
base: master
head: short_long_help
created_at: 2017-04-05T05:27:20Z
updated_at: 2017-04-05T15:38:59Z
url: https://github.com/BurntSushi/ripgrep/pull/438
synced_at: 2026-01-12T18:23:13Z
```

# updates clap and removes home rolled -h/--help distinction

---

_@kbknapp_

~~**DO NOT MERGE - until clap v2.23.1 is on crates.io**~~

It's safe to merge now.

- [x] crates.io needs to contain clap v2.23.1
- [x] I need to rebase the `Cargo.lock`

This commit updates clap to v2.23.1

The update contained a bug fix in clap that results in broken code in
ripgrep. ripgrep was unknowingly relying on the bug, but this commit fixes that
issue. The bug centered around not being able to override the
auto-generated help message by supplying a flag with a long of `help`.

Normally, supplying a flag with a long of `help` means whenever the user
passes `--help`, the consuming code (e.g. ripgrep) is responsible for
displaying the help message. However, due to the bug in clap this wasn't
necessary for ripgrep to do unless the user passed `-h`. With the bug
fixed, it meant the user passing `--help` and clap expected ripgrep to
display the help, yet ripgrep expected clap to display the help. This
has been fixed in this commit of ripgrep.

All is well now!

v2.23.0 also brings the abilty to use `Arg::help` or `Arg::long_help`
allowing one to distinguish between `-h` and `--help`. This commit
leaves all doc strings in the `lazy_static!` hashmap however only for
aesthetic reasons.

This means all home rolled handling of `-h`/`--help` has been removed
from ripgrep, yet functionality *and* appearances are 100% the same.

---

_Comment by @BurntSushi on 2017-04-05 11:16_

*Excellent!* Thanks so much for doing this. I much appreciate the simpler case analysis. :-)

At some point, I'll get rid of the `USAGES` map and inline the docs, but I like that this PR doesn't do that. :-)

---

_Renamed from "updates clap and removes home rolled -h/--help distinction (DO NOT MERGE YET)" to "updates clap and removes home rolled -h/--help distinction" by @kbknapp on 2017-04-05 15:04_

---

_Merged by @BurntSushi on 2017-04-05 15:38_

---

_Closed by @BurntSushi on 2017-04-05 15:38_

---
