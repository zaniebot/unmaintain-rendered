```yaml
number: 97
title: Avoid re-indexing when new files are added
type: issue
state: open
author: MichaReiser
labels:
  - cli
  - performance
assignees: []
created_at: 2025-05-04T11:03:42Z
updated_at: 2025-06-09T16:57:59Z
url: https://github.com/astral-sh/ty/issues/97
synced_at: 2026-01-12T15:54:22Z
```

# Avoid re-indexing when new files are added

---

_@MichaReiser_

ty re-indexes all project files when a new file is added because it currently has no other way to determine if the file is ignored by a .gitignore (or some other rule). This is wasteful and can make ty unnecessarily slow when switching between commits. 

We should explore a cheaper way to determine if a file is excluded that doesn't require traversing an entire sub-tree.

---

_Label `cli` added by @MichaReiser on 2025-05-04 11:03_

---

_Comment by @Skylion007 on 2025-05-04 18:56_

@MichaReiser For `.gitignore` specifically there is https://git-scm.com/docs/git-check-ignore which I am sure has rust bindings.

---

_Comment by @MichaReiser on 2025-05-04 18:58_

Thanks @Skylion007 We use the `ignore` crate for traversing and finding the files. I think it also has some APIs to test individual files. It would be best to use the `ignore` file to ensure we get consistent results between incremental and initial indexing.

---

_Comment by @Skylion007 on 2025-05-04 19:01_

@MichaReiser The API you are looking for is `matcher::matched` https://docs.rs/ignore/latest/ignore/gitignore/struct.Gitignore.html#method.matched

```rust
let matcher = WalkBuilder::new(".")
    .standard_filters(true)
    .build_ignore();

let path = Path::new("my/project/root/src/foo.rs");
matcher.matched(path, false);  // false = treat as file
```

---

_Label `cli` added by @MichaReiser on 2025-05-07 15:17_

---

_Label `performance` added by @AlexWaygood on 2025-05-10 18:01_

---

_Comment by @MichaReiser on 2025-06-09 16:53_

I think we would have to use [`matched_path_or_any_parents`](https://docs.rs/ignore/latest/ignore/gitignore/struct.Gitignore.html#method.matched_path_or_any_parents) and the "hard part" is to collect all ignore files for a given file. Maybe @BurntSushi has an idea on how this could be done with the ignore crate.

I suspect that it will require us to build something custom (or create an adhoc `WalkBuilder` as you suggest which is sort of expensive and I'm not sure if it accounts for ancestor ignore files?)

---

_Comment by @BurntSushi on 2025-06-09 16:57_

Yeah you can't really do this with `ignore`. `ignore` isn't designed to deal with one file path at a time. It's designed only for doing entire traversals. It basically has two halves: a very low-level API for parsing and matching a single gitignore file and a very high-level API for doing a full directory traversal. There's really nothing inbetween those two.

---
