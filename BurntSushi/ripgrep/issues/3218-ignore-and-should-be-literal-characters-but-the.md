```yaml
number: 3218
title: "ignore: `{` and `}` should be literal characters, but the `ignore` crate treates them as glob character"
type: issue
state: closed
author: j178
labels: []
assignees: []
created_at: 2025-11-12T08:54:45Z
updated_at: 2025-11-12T11:43:17Z
url: https://github.com/BurntSushi/ripgrep/issues/3218
synced_at: 2026-01-12T16:13:25Z
```

# ignore: `{` and `}` should be literal characters, but the `ignore` crate treates them as glob character

---

_@j178_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ignore 0.4.25

### How did you install ripgrep?

n/a

### What operating system are you using ripgrep on?

Darwin 24.5.0 arm64

### Describe your bug.

In `.gitignore` [pattern syntax](https://git-scm.com/docs/gitignore#_pattern_format), `{` and `}` are just literal characters, but the `ignore` crate treates them as glob character.

### What are the steps to reproduce the behavior?

An exmaple project:

```console
$ git init /tmp/example
$ cd /tmp/example/
$ echo '{f}' > .gitignore
$ touch '{f}'
```

### What is the actual behavior?

The `{f}` is listed by the below code:

```rust
// List files that are not ignored by .gitignore using the ignore crate.
fn main() -> Result<(), Box<dyn std::error::Error>> {
    let path = std::env::args().nth(1).unwrap_or_else(|| ".".to_string());
    for result in ignore::WalkBuilder::new(path).build() {
        let entry = result?;
        if entry.file_type().is_some_and(|f| f.is_file()) {
            println!("{}", entry.file_name().display());
        }
    }
    Ok(())
}

```

```console
$ cargo run -q -- /tmp/example/ # `{f}` is not ignored
{f}
```


### What is the expected behavior?

`{f}` file should be ignored, like what git does:

```console
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.gitignore

nothing added to commit but untracked files present (use "git add" to track)
```

---

_Closed by @BurntSushi on 2025-11-12 11:43_

---
