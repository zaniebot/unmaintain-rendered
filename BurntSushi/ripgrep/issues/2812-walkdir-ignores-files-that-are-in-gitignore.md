```yaml
number: 2812
title: WalkDir ignores files that are in .gitignore outside of current git repository
type: issue
state: closed
author: RudolfVonKrugstein
labels:
  - wontfix
assignees: []
created_at: 2024-05-20T14:12:23Z
updated_at: 2024-05-20T14:44:17Z
url: https://github.com/BurntSushi/ripgrep/issues/2812
synced_at: 2026-01-12T16:13:24Z
```

# WalkDir ignores files that are in .gitignore outside of current git repository

---

_@RudolfVonKrugstein_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

git clone of master (f1d23c06e30606b2428a4e32da8f0b5069e81280)

### How did you install ripgrep?

cargo/I used the source

### What operating system are you using ripgrep on?

Ubuntu in WSL2

### Describe your bug.

When WalkDir is created via `WalkBuilder` and `reguire_git(false)` enabled, files that are ignored by an `.gitignore` above the current git repository root directory, are ignored (also git would not ignore them). When `require_git(treu)` is enabled, this is not the case.

### What are the steps to reproduce the behavior?

1. Create this test applicatin:

```rust
use std::env;

use ignore::WalkBuilder;

fn main() {
    let require_git_arg = if env::args().nth(1).unwrap() == "true" {
        true
    } else { false };

    let walker = WalkBuilder::new(".").require_git(require_git_arg).build();
    for result in walker {
        println!("{:?}", result.unwrap())
    }
}
```

2. Create a git repository with a file in it:

```sh
git init
touch test
```

3. Create a .gitignore outside of the git repoistory: 

```sh
echo "test\n" > ../.gitignore
```

4. run the application with `require_git(true)` and see, that the `.gitignore` is not considered:

```sh
cargo run -- true
DirEntry { dent: Walkdir(DirEntry(".")), err: None }                                                                                                                                                                                                        DirEntry { dent: Walkdir(DirEntry("./test")), err: None }  
```

5. run the application with `require_git(rqlw3)` and see, that the `.gitignore` is considered:

```sh
cargo run -- true
DirEntry { dent: Walkdir(DirEntry(".")), err: None }                                                                                                                                                                                                       
```

### What is the actual behavior?

`WalkDir`, created from `WalkBuilder` with`require_git(false)` uses any `.gitignore` files, that are outside of the current git repo (and therefor are not used by git) while `require_git(true)` does not use them.

### What is the expected behavior?

The `.gitignore` outside/above the current git reo should not be used, regardless if `require_git` is called with `true` or `false`.

---

_Comment by @BurntSushi on 2024-05-20 14:17_

> The `.gitignore` outside/above the current git reo should not be used, regardless if `require_git` is called with `true` or `false`.

That's the entire point of `require_git`: it ignores whether `.git` is present or not and just uses every `.gitignore` that it finds. If you don't want ripgrep to cross git repository boundaries, then you should enable `require_git`.

---

_Closed by @BurntSushi on 2024-05-20 14:17_

---

_Label `wontfix` added by @BurntSushi on 2024-05-20 14:17_

---

_Comment by @RudolfVonKrugstein on 2024-05-20 14:38_

Thank you for your reply. Just for completenes my perspective and how I got to creating this issue:

I thought the point of `require_git(false)` is to apply gitignore rules even when there is no git repository. I did not expect `require_git(false)` to behave differently from `require_git(true)` when there is a git repository.

---
