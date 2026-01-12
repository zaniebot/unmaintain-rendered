```yaml
number: 2644
title: Add -d as short command for --max-depth
type: pull_request
state: closed
author: 3tilley
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2023-11-02T19:53:18Z
updated_at: 2023-11-21T23:39:37Z
url: https://github.com/BurntSushi/ripgrep/pull/2644
synced_at: 2026-01-12T18:23:14Z
```

# Add -d as short command for --max-depth

---

_@3tilley_

I've given a go to #2643, partly just as an experiment into how easy it would be - the answer was pretty easy with the tools available.

I've not:
* Updated much documentation. The help is generated automatically, and I followed the model of `--max-count / -m` while making this change, where it's exclusively referred to by the long name
* Added or updated any tests - I couldn't see that the `--max-depth` functionality is tested anywhere, so I didn't see anywhere to add or tweak for the short form. I can add some if you would like

I have given my best go at updating the zsh completions, but I'd be lying if I said I understood exactly how it worked. I've followed the example of `--max-count` and `--max-columns`, but I'd appreciate your eye to make sure it's been implemented correctly.

I've also tested locally, and it seems to work the same as `--max-depth` does:

```
$./target/debug/rg.exe glue --max-depth 4
$./target/debug/rg.exe glue -d 4
$./target/debug/rg.exe glue -d4
$./target/debug/rg.exe glue -d=4
$./target/debug/rg.exe glue --max-depth 5
crates\searcher\src\searcher\mod.rs
20:    searcher::glue::{MultiLine, ReadByLine, SliceByLine},
27:mod glue;
$./target/debug/rg.exe glue -d 5
crates\searcher\src\searcher\mod.rs
20:    searcher::glue::{MultiLine, ReadByLine, SliceByLine},
27:mod glue;
$./target/debug/rg.exe glue -d5
crates\searcher\src\searcher\mod.rs
20:    searcher::glue::{MultiLine, ReadByLine, SliceByLine},
27:mod glue;
$./target/debug/rg.exe glue -d=5
crates\searcher\src\searcher\mod.rs
20:    searcher::glue::{MultiLine, ReadByLine, SliceByLine},
27:mod glue;
```

If you agree with the premise of the change, let me know if there is anything I should update or check for.

Thanks!

---

_Label `rollup` added by @BurntSushi on 2023-11-21 23:10_

---

_Closed by @BurntSushi on 2023-11-21 23:39_

---
