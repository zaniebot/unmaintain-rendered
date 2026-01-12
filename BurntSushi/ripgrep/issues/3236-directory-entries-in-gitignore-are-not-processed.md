```yaml
number: 3236
title: "Directory entries in `.gitignore` are not processed in the same way as in Git"
type: issue
state: open
author: Cerber-Ursi
labels: []
assignees: []
created_at: 2025-12-04T13:57:30Z
updated_at: 2025-12-04T14:17:03Z
url: https://github.com/BurntSushi/ripgrep/issues/3236
synced_at: 2026-01-12T16:13:25Z
```

# Directory entries in `.gitignore` are not processed in the same way as in Git

---

_@Cerber-Ursi_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

Not a ripgrep itself, but `ignore v0.4.25`; reproduced at [current `master` branch](https://github.com/BurntSushi/ripgrep/commit/cd1f981beafaeb9b61537e47e91314cea125400b) at the moment of writing.

### How did you install ripgrep?

Just using `ignore` from crates.io.

### What operating system are you using ripgrep on?

Arch, rolling release

### Describe your bug.

When `.gitignore` file contains a literal directory reference, `ignore` still includes contents of that directory. This is in contradiction with git itself, which ignores this directory.

### What are the steps to reproduce the behavior?

Reproduces with the following test in `ignore`:
```rust
#[test]
fn gitignore_directory() {
    let td = tmpdir();
    mkdirp(td.path().join("a"));
    wfile(td.path().join(".gitignore"), "/a");
    wfile(td.path().join("a/foo"), "");

    let root = td.path();
    assert_paths(&root, &WalkBuilder::new(&root), &[]);
}
```

Real-life case: repository with a single Rust project, where .gitignore file is as following:
```
/target
```

### What is the actual behavior?

Files in the `.gitignore`d directory are included in a walk.

### What is the expected behavior?

Files in the `.gitignore`d directory are ignored, as they do when using git.

---

_Comment by @Cerber-Ursi on 2025-12-04 14:02_

Reproduced even simpler, directly in the `gitignore.rs`:
```rust
ignored!(ig45, ROOT, "/foo", "foo/bar"); // fails
```

---

_Comment by @BurntSushi on 2025-12-04 14:03_

By default, `ignore` requires that there is an actual git repository in order to respect `.gitignore`. But that can be disabled with [`WalkBuilder::require_git`](https://docs.rs/ignore/latest/ignore/struct.WalkBuilder.html#method.require_git). It looks like your test doesn't have a git repository in the directory?

---

_Comment by @BurntSushi on 2025-12-04 14:03_

> Reproduced even simpler, directly in the `gitignore.rs`:
> 
> ignored!(ig45, ROOT, "/foo", "foo/bar"); // fails

The `gitignore.rs` API is lower level. That test result is correct.

---

_Comment by @Cerber-Ursi on 2025-12-04 14:17_

> ignore requires that there is an actual git repository in order to respect .gitignore.

Aha, thanks - that's something I've missed. Simplified use case is indeed a false alarm, but real use case was actually more like this:
```rust
#[test]
fn gitignore_parent_ignoring_directory() {
    let td = tmpdir();
    mkdirp(td.path().join(".git"));
    mkdirp(td.path().join("a/foo"));
    wfile(td.path().join(".gitignore"), "/a");
    wfile(td.path().join("a/foo/bar"), "");

    let root = td.path().join("a/foo");
    assert_paths(td.path(), &WalkBuilder::new(&root), &[]);
}
```
...that is, I'm somewhere inside the repository already, and `gitignore` excludes the path passed to `WalkBuilder`. Is this case expected to work as it does now?

---
