```yaml
number: 303
title: Provide a mechanism to compose type definitions
type: pull_request
state: closed
author: isker
labels: []
assignees: []
base: master
head: master
created_at: 2017-01-05T23:46:43Z
updated_at: 2017-01-07T23:15:53Z
url: https://github.com/BurntSushi/ripgrep/pull/303
synced_at: 2026-01-12T18:23:12Z
```

# Provide a mechanism to compose type definitions

---

_@isker_

This extends the syntax of the --type-add flag to allow including the globs of
other already defined types.

Fixes #83.

Hi @BurntSushi, this is pretty much a straight implementation of your spec from #83, except that we are also allowing Unicode numbers in type names in addition to Unicode letters.  Letters were not enough for the default types (curse you, m4).

I am no rust expert so I'd appreciate specific feedback on my add_def changes (or anything, for that matter).  And let me know if I missed any places to put tests or documentation (or if I put the documentation in too many places; it did feel a bit repetitive).

Thanks!

---

_@BurntSushi reviewed on 2017-01-05 23:57_

---

_Review comment by @BurntSushi on `ignore/src/types.rs` on 2017-01-05 23:57_

Sorry, but this is going to fail CI because ripgrep's minimum version is currently Rust 1.12. You'll need to use `try!` here.

---

_@BurntSushi reviewed on 2017-01-06 00:00_

---

_Review comment by @BurntSushi on `src/app.rs`:461 on 2017-01-06 00:00_

These docs are great. Can you also add them to the man page in `doc/rg.1.md`? (You'll need to `cd doc && ./convert-to-man` to actually produce the `man` file, which can be viewed with `man -l rg.1`. If you can't get `pandoc` installed easily, that's OK, I can do the final conversion step.)

---

_@BurntSushi requested changes on 2017-01-06 00:01_

This looks fantastic! Thank you so much! I have just a couple of minor nits, but this should otherwise be good to go. :D

---

_Comment by @BurntSushi on 2017-01-06 13:19_

Oh, could you also add an integration test in `tests/tests.rs`? Thanks!

---

_Comment by @isker on 2017-01-07 21:52_

I was able to install pandoc but I could not view the man file.  Probably some OSX incompatibility?  Either way, you might want to take a look yourself.

---

_Comment by @BurntSushi on 2017-01-07 23:15_

Merged in https://github.com/BurntSushi/ripgrep/commit/ed01e80a796b7f5cbc83c63517d884d85f1cebd7

---

_Closed by @BurntSushi on 2017-01-07 23:15_

---

_Comment by @BurntSushi on 2017-01-07 23:15_

Thanks! The doc formatting was a little off. I fixed that and also squashed the commits. Nice work!

---
