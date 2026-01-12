```yaml
number: 218
title: adding cs filetype for ease of use.
type: pull_request
state: merged
author: theamazingfedex
labels: []
assignees: []
merged: true
base: master
head: adding-cs-filetype
created_at: 2016-11-03T21:46:12Z
updated_at: 2016-11-04T00:46:03Z
url: https://github.com/BurntSushi/ripgrep/pull/218
synced_at: 2026-01-12T18:23:12Z
```

# adding cs filetype for ease of use.

---

_@theamazingfedex_

_No description provided._

---

_Comment by @BurntSushi on 2016-11-03 21:48_

What file type is this?


---

_Comment by @theamazingfedex on 2016-11-03 21:54_

.cs for csharp


---

_Comment by @BurntSushi on 2016-11-03 21:56_

OK, so you're proposing that we have two names for the same file type. I'd like to avoid this, but we do have precedent for it, so I guess we can merge.


---

_Comment by @theamazingfedex on 2016-11-03 22:06_

I'm not sure if Rust will allow for this, but if there were a way to have multiple keys for each type, we could avoid having two distinct entries for it. 
Probably something like the following:

``` Rust
const DEFAULT_TYPES: &'static [(&'static [&'static str], &'static [&'static str])] = &[
  (&["cs", "csharp"], &["*.cs"]),
  (&["md", "markdown"], &["*.markdown", "*.md", "*.mdown", "*.mkdn"])
]
```

If this is possible, it would also require some slight refactoring of the rg type checking module.


---

_Comment by @BurntSushi on 2016-11-03 22:12_

I'm actually fine with the duplication in source code for now. What concerns me more is the UX. That is, having multiple ways to refer to the same thing. (With that said, of course your suggestion is better and that looks like valid Rust to me, but it's fine to leave it as is for now.)

> If this is possible, it would also require some slight refactoring of the rg type checking module.

At least the `Type` matcher itself could be made to duplicate the definitions, even if they aren't duplicated in the source code, so I think that's fine.


---

_Merged by @BurntSushi on 2016-11-04 00:46_

---

_Closed by @BurntSushi on 2016-11-04 00:46_

---
