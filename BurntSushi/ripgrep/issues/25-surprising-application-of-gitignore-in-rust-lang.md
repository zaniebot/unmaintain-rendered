```yaml
number: 25
title: Surprising application of .gitignore in rust-lang/rust repo
type: issue
state: closed
author: brson
labels:
  - bug
assignees: []
created_at: 2016-09-23T18:30:56Z
updated_at: 2016-09-24T22:40:53Z
url: https://github.com/BurntSushi/ripgrep/issues/25
synced_at: 2026-01-12T18:23:11Z
```

# Surprising application of .gitignore in rust-lang/rust repo

---

_@brson_

This may be working as designed but it was quite surprising. If I run `rg LLVM_BINDINGS` from the top level of the rust repo I get results. If I do the same from `src/` I get no results. The reason is that rust's .gitignore file contains `/llvm/`.

Perhaps this is how .gitignore is defined to work, but it's not what I expected. I might expect that .gitignore would by applied relative to the directory in which it is defined.


---

_Comment by @foophoof on 2016-09-23 18:50_

According to `gitignore(5)` on my system (OS X running Git 2.9.0) (emphasis mine):

> - Patterns read from a .gitignore file in the same directory as the path, or in any parent directory, with patterns in the higher level files (up to the toplevel of the work tree) being overridden by those in lower level files down to the directory containing the file. **These patterns match relative to the location of the .gitignore file.** A project normally includes such .gitignore files in its repository, containing patterns for files generated as part of the project build.

So I think your expectation is correct, that it _should_ match relative to the location of the file, not the current directory.


---

_Label `bug` added by @BurntSushi on 2016-09-24 02:13_

---

_Comment by @BurntSushi on 2016-09-24 02:13_

It is indeed a plain ol' bug.


---

_Closed by @BurntSushi on 2016-09-24 22:40_

---
