```yaml
number: 2986
title: "[`ignore`] `Walk` always yields initial directory"
type: issue
state: closed
author: wmmc88
labels:
  - wontfix
assignees: []
created_at: 2025-02-05T02:27:52Z
updated_at: 2025-11-05T21:04:53Z
url: https://github.com/BurntSushi/ripgrep/issues/2986
synced_at: 2026-01-12T16:13:25Z
```

# [`ignore`] `Walk` always yields initial directory

---

_@wmmc88_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ignore 0.4.23

### How did you install ripgrep?

Dependency on `ignore` via `cargo`

### What operating system are you using ripgrep on?

Windows 11 24H2

### Describe your bug.

When creating a `Walk`, the iterator always returns the initial folder, even if it should fail `filter_entry`. It looks like its bypassing `filter_entry` altogether for the intial folder.

### What are the steps to reproduce the behavior?

The following minimal example code manifests the bug:

```rust
use std::path::Path;

use ignore::WalkBuilder;

fn main(){
    let initial_folder = dbg!(Path::new(file!()).ancestors().nth(2).expect("2nd ancestor should exist").join("example-folder").canonicalize().expect("example-folder path should exist"));

    WalkBuilder::new(&initial_folder)
        .filter_entry(|dir_entry| {
            let is_file = dir_entry
                .file_type().is_some_and(|file_type| file_type.is_file());
            dbg!(&dir_entry, is_file);

            is_file
        })
        .build().for_each(|filtered_entry| {
            dbg!(&filtered_entry);
        });
}
```

It is also available as a minimal cargo workspace at https://github.com/wmmc88/minimal-repros/tree/ignore-walkbuilder

### What is the actual behavior?

As seen in the output, the initial folder is unconditionally yield by the iterator (in `filtered_entry`), without ever executing the `filter_entry` closure (which prints `&dir_entry`). The iterator still calls the closure properly on the `nested` and then `nested` correctly isn't yielded by `Walk`.

```
cargo run
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.28s
     Running `target\debug\ignore-bug.exe`
[src\main.rs:6:26] Path::new(file!()).ancestors().nth(2).expect("2nd ancestor should exist").join("example-folder").canonicalize().expect("example-folder path should exist") = "\\\\?\\D:\\git-repos\\github\\minimal-repros\\example-folder"
[src\main.rs:17:13] &filtered_entry = Ok(
    DirEntry {
        dent: Walkdir(
            DirEntry("\\\\?\\D:\\git-repos\\github\\minimal-repros\\example-folder"),
        ),
        err: None,
    },
)
[src\main.rs:12:13] &dir_entry = DirEntry {
    dent: Walkdir(
        DirEntry("\\\\?\\D:\\git-repos\\github\\minimal-repros\\example-folder\\a.txt"),
    ),
    err: None,
}
[src\main.rs:12:13] is_file = true
[src\main.rs:17:13] &filtered_entry = Ok(
    DirEntry {
        dent: Walkdir(
            DirEntry("\\\\?\\D:\\git-repos\\github\\minimal-repros\\example-folder\\a.txt"),
        ),
        err: None,
    },
)
[src\main.rs:12:13] &dir_entry = DirEntry {
    dent: Walkdir(
        DirEntry("\\\\?\\D:\\git-repos\\github\\minimal-repros\\example-folder\\b.txt"),
    ),
    err: None,
}
[src\main.rs:12:13] is_file = true
[src\main.rs:17:13] &filtered_entry = Ok(
    DirEntry {
        dent: Walkdir(
            DirEntry("\\\\?\\D:\\git-repos\\github\\minimal-repros\\example-folder\\b.txt"),
        ),
        err: None,
    },
)
[src\main.rs:12:13] &dir_entry = DirEntry {
    dent: Walkdir(
        DirEntry("\\\\?\\D:\\git-repos\\github\\minimal-repros\\example-folder\\c.txt"),
    ),
    err: None,
}
[src\main.rs:12:13] is_file = true
[src\main.rs:17:13] &filtered_entry = Ok(
    DirEntry {
        dent: Walkdir(
            DirEntry("\\\\?\\D:\\git-repos\\github\\minimal-repros\\example-folder\\c.txt"),
        ),
        err: None,
    },
)
[src\main.rs:12:13] &dir_entry = DirEntry {
    dent: Walkdir(
        DirEntry("\\\\?\\D:\\git-repos\\github\\minimal-repros\\example-folder\\nested"),
    ),
    err: None,
}
[src\main.rs:12:13] is_file = false
```

### What is the expected behavior?

The `Walk` iterator should have executed `filter_entry`'s closure on `initial_folder`, and not yielded it.

---

_Comment by @BurntSushi on 2025-02-05 02:37_

Thanks for the detailed reproduction, but this is very much intended. `ignore` is how ripgrep does directory traversal, and if you explicitly provide a path on the CLI, then ripgrep always searches it, regardless of what filtering rules are present. That is, explicitly providing a path _overrides_ the filtering rules.

---

_Closed by @BurntSushi on 2025-02-05 02:37_

---

_Label `wontfix` added by @BurntSushi on 2025-02-05 02:37_

---

_Comment by @wmmc88 on 2025-02-05 03:22_

Ah okay. I think it'd be beneficial if that override behavior was documented somewhere (I couldnt find it anywhere but maybe I missed it?)

---

_Comment by @gospodima on 2025-11-05 20:56_

@BurntSushi could you give any suggestions on how to overcome this behaviour in a clean manner? I am trying to use `ignore` in the tool that is also supposed to be usable via [pre-commit](https://pre-commit.com/). pre-commit would pass all staged files as input paths and I still want to apply exclusion rules on them

---

_Comment by @BurntSushi on 2025-11-05 21:04_

@gospodima You'd probably need to apply the filtering yourself directly with `ignore::gitignore::Gitignore` or `ignore::overrides::Override`. It may not be straight-forward or easy.

---
