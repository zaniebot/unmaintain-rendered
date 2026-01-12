```yaml
number: 3150
title: "[ignore] GitignoreBuilder::new(&path).build_global() fails to match any files if global .gitignore missing"
type: issue
state: closed
author: Reeywhaar
labels:
  - invalid
assignees: []
created_at: 2025-09-20T03:27:31Z
updated_at: 2025-09-21T16:47:15Z
url: https://github.com/BurntSushi/ripgrep/issues/3150
synced_at: 2026-01-12T16:13:25Z
```

# [ignore] GitignoreBuilder::new(&path).build_global() fails to match any files if global .gitignore missing

---

_@Reeywhaar_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

I use `ignore` package: 0.4.23

### How did you install ripgrep?

I use `ignore` package, not ripgrep itself

### What operating system are you using ripgrep on?

MacOs 15.6.1

### Describe your bug.

Considering default setup:
```sh
#: git config --get core.excludesFile
empty...
```

```rust
dbg!(gitconfig_excludes_path());
```

```sh
/Users/%user%/config/git/ignore # result of the call even though the file doesn't exist!
```

If we try to run `Gitignore` built with `GitignoreBuilder::new(&some_repo_path).build_global()` in any git directory it will fail to match any file that is ignored.

Instead we should check manually if global gitignore file exists
```sh
let mut gitignore_builder = GitignoreBuilder::new(&self.path);
if let Some(gitconfig_path) = gitconfig_excludes_path() {
    if gitconfig_path.exists() {
        let err = gitignore_builder.add(gitconfig_path);
        if let Some(err) = err {
            return Err(err.into());
        }
    }
}
let err = gitignore_builder.add(self.path.join(".gitignore"));
if let Some(err) = err {
    return Err(err.into());
}
let gitignore = gitignore_builder.build()?;
```

### What are the steps to reproduce the behavior?

Remove global gitignore file from git settings and test `GitignoreBuilder::new(&some_repo_path).build_global()` on any directory you expect to have git ignored files

### What is the actual behavior?

```rs
let gitignore_builder = GitignoreBuilder::new(&some_repo_path);
let gitignore = gitignore_builder.build_global().unwrap()
gitignore.matched(&any_ignored_file, is_dir).is_ignore(); // false
```

### What is the expected behavior?

```rs
gitignore.matched(&any_ignored_file, is_dir).is_ignore(); // should be true
```

---

_Renamed from "[ignore] GitignoreBuilder::new(&path).build_global() fails to find any files if global .gitignore missing" to "[ignore] GitignoreBuilder::new(&path).build_global() fails to match any files if global .gitignore missing" by @Reeywhaar on 2025-09-20 03:33_

---

_Comment by @BurntSushi on 2025-09-20 10:45_

I can't make heads or tails of this bug report. Please provide a *complete* minimal reproducible example.

---

_Comment by @Reeywhaar on 2025-09-20 19:13_

Made a repo that runs tests in docker on vanilla debian:bookworm with no predefined global gitignore.
`sh scripts/run_test.sh`

https://github.com/Reeywhaar/ripgrep-3150

Main concern is that `case_with_global` fails

---

_Comment by @BurntSushi on 2025-09-21 16:34_

As far as I can tell, you're misusing `Gitignore`. A `Gitignore` represents a _single_ set of gitignore rules. But you seem to try to be using it as if it takes multiple sets of gitignore rules into account. It doesn't.

If you're using `Gitignore` directly, then you have to re-implement gitignore filtering precedence yourself. This is what the recursive file walker does for you. It is non-trivial.

---

_Closed by @BurntSushi on 2025-09-21 16:34_

---

_Label `invalid` added by @BurntSushi on 2025-09-21 16:34_

---

_Comment by @Reeywhaar on 2025-09-21 16:43_

Oh, ok, thought it takes multiple `.gitignore` files and merges them. Even though you can see that slight modification of `GitignoreBuilder` init that is seemingly equal results in different behavior which is confusing. Nevermind then, thank you so much, I'll try `Walk` instead!

---

_Comment by @BurntSushi on 2025-09-21 16:47_

Yes, the API, especially the lower level bits, is extremely confusing.

---
