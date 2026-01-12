```yaml
number: 3179
title: "ignore::WalkBuilder.git_global(true) does not behave correctly with rooted files"
type: issue
state: closed
author: Yutsuten
labels:
  - bug
assignees: []
created_at: 2025-10-12T10:44:13Z
updated_at: 2025-10-15T23:44:25Z
url: https://github.com/BurntSushi/ripgrep/issues/3179
synced_at: 2026-01-12T16:13:25Z
```

# ignore::WalkBuilder.git_global(true) does not behave correctly with rooted files

---

_@Yutsuten_

This bug was originally reported in the helix repository: https://github.com/helix-editor/helix/issues/12604

I was doing a bit of a research in that issue, and I found that helix [is using the package `ignore`](https://github.com/helix-editor/helix/blob/5b0563419eeeaf0595c848865c46be4abad246a7/helix-term/Cargo.toml#L73), and we can see instances of `WalkBuilder` in the file picker function [here](https://github.com/helix-editor/helix/blob/5b0563419eeeaf0595c848865c46be4abad246a7/helix-term/src/ui/mod.rs#L234).

The full explanation of the bug can be read there, but the TLDR is: if the global `.gitignore`, specified in git's config `core.excludesfile` option contains a rooted pattern, for example `/.venv`, it is **not** ignored as expected.

I looked for similar issues to see if anyone have reported it already, but I was unable to find any.

---

_Comment by @BurntSushi on 2025-10-12 10:56_

Please provide an MRE.

---

_Comment by @Yutsuten on 2025-10-12 14:22_

I was able to reproduce the issue with the code bellow:

```rust
use ignore::WalkBuilder;
use std::env;

fn main() {
    let root = env::current_dir().unwrap();
    println!("Root is {:?}", root);
    let mut walk_builder = WalkBuilder::new(root);
    let walk = walk_builder
        .hidden(false)
        .git_ignore(true)
        .git_global(true)
        .build();
    for dir_entry in walk {
        println!("{:?}", dir_entry.unwrap().path());
    }
}
```

And created a dummy `.venv` folder with two files:

```shell
mkdir .venv
touch .venv/foo
touch .venv/bar
```

With `/.venv` ignored in the global ignore file, `.venv` entries are printed:

```txt
"/home/mateus/Projects/ripgrep-issue/.venv"
"/home/mateus/Projects/ripgrep-issue/.venv/bar"
"/home/mateus/Projects/ripgrep-issue/.venv/foo"
```

If I copy/move the `/.venv` entry from the global ignore to my project's `.gitignore` (in my case, `~/Projects/ripgrep-issue/.gitignore`) and run again, `.venv` is properly ignored and not printed.

At the beginning I was using `let root = ".";` and was unable to reproduce the issue. I think this is an important clue, because it seems that the bug is only triggered when using a full path as a root.

Now, if this behavior is intended or not in `ripgrep`, why Helix needs to use the full path as root, I don't know.
If this is intended, could you tell me if there is any function/option we could enable to make global gitignore work even when using full path root?

---

_Label `bug` added by @BurntSushi on 2025-10-15 22:16_

---

_Comment by @BurntSushi on 2025-10-15 22:17_

Thanks for reporting this! This was gnarly to fix. And this bug appeared in ripgrep as well. Moreover, the bug occurs with `WalkBuilder::add_ignore`. I have a fix in #3189.

---

_Closed by @BurntSushi on 2025-10-15 23:44_

---
