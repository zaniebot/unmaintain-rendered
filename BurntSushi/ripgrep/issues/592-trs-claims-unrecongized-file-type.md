```yaml
number: 592
title: "`-trs` claims unrecongized file type"
type: issue
state: closed
author: valarauca
labels: []
assignees: []
created_at: 2017-09-01T17:35:50Z
updated_at: 2017-09-01T18:00:57Z
url: https://github.com/BurntSushi/ripgrep/issues/592
synced_at: 2026-01-12T16:13:22Z
```

# `-trs` claims unrecongized file type

---

_@valarauca_

Running ripgrep to find a debug message, within the root I run `$ rg -trs 'ProcNotes'` to help locate a debug error. I was notified `unrecongized file type: .rs`.

I rebuilt the current git repo's head, and the error persisted. Am I doing something wrong?

---

_Comment by @okdana on 2017-09-01 17:38_

The proper name of the type that includes `.rs` files is actually `rust`:

```
% rg --type-list | rg -w rs
rust: *.rs
```

So `-trust` should work.

---

_Comment by @BurntSushi on 2017-09-01 17:38_

I think the error message pretty much says it. `rs` isn't a recognized file type. Assuming you're trying to search Rust files, I think you want `-trust`. If you want to see the available file types, you can run `rg --type-list`.

---

_Comment by @BurntSushi on 2017-09-01 17:38_

I wouldn't be opposed to adding `rs`, although I'm trying to avoid having a bunch of aliases (but am failing).

---

_Comment by @valarauca on 2017-09-01 18:00_

I just kind of assumed as `-ttoml` or `-tjs` seemed to imply `-trs`. 

>I'm trying to avoid having a bunch of aliases (but am failing).

Welcome to building a cli tool :P 

---

_Closed by @valarauca on 2017-09-01 18:00_

---
