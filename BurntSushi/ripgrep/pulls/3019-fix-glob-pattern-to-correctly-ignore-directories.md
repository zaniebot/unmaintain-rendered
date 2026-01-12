```yaml
number: 3019
title: Fix glob pattern to correctly ignore directories with only dots
type: pull_request
state: closed
author: pi55man
labels: []
assignees: []
base: master
head: fix-dots
created_at: 2025-03-29T16:32:28Z
updated_at: 2025-08-17T21:26:26Z
url: https://github.com/BurntSushi/ripgrep/pull/3019
synced_at: 2026-01-12T18:23:14Z
```

# Fix glob pattern to correctly ignore directories with only dots

---

_@pi55man_

This PR addresses [issue #2990](https://github.com/BurntSushi/ripgrep/issues/2990), where `ripgrep` did not properly ignore directories named only with dots (e.g., `.`, `..`, `...`) when using negated glob patterns (`-g '!dir'`).  

Previously, the `file_name` function in `globset` only checked if the last byte was a dot (`.`), which led to incorrect behavior. This caused:  
- `rg --files -g '!asdf.'` still listing `asdf./foo`  
- But `rg --files -g '!asdf'` correctly ignoring `asdf/foo`  

### Fix  
The function now ensures that entire filenames made only of dots are ignored by using:  
```rust
if path.iter().all(|&b| b == b'.') {
    return None;
}


---

_Comment by @BurntSushi on 2025-08-17 21:26_

Thank you for looking into this!

The status quo was certainly wrong, but this isn't quite right either. This is checking the entire path, which does more work than is necessary (although it is likely to short circuit very early on most paths). It's also not quite the semantics I was going for.

I wanted to [imitate the corresponding routine on `std::path::Path`](https://doc.rust-lang.org/std/path/struct.Path.html#method.file_name), but it looks like I flubbed it.

What this should be doing is checking if the entire file name is `..`, and if it is, returning `None`.

Also, for changes like this, it is customary to include a regression test.

---

_Closed by @BurntSushi on 2025-08-17 21:26_

---
