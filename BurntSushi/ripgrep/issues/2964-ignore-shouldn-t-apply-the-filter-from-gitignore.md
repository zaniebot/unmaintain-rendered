```yaml
number: 2964
title: "ignore shouldn't apply the filter from .gitignore to .git folder."
type: issue
state: closed
author: gwen-lg
labels:
  - wontfix
assignees: []
created_at: 2025-01-07T20:00:31Z
updated_at: 2025-09-23T01:25:22Z
url: https://github.com/BurntSushi/ripgrep/issues/2964
synced_at: 2026-01-12T16:13:25Z
```

# ignore shouldn't apply the filter from .gitignore to .git folder.

---

_@gwen-lg_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ignore v0.4.23

### How did you install ripgrep?

Cargo

### What operating system are you using ripgrep on?

Fedora 41

### Describe your bug.

If I use ignore to copy a folder with the associate git repository files, the filters of `.gitignore` are applied to files of the .git folder (ex with .idx).
Files from a .git folder work as a whole, it's seems logical to not apply to them the filter of `.gitignore` (when hidden is set to false in Walk)

Related issue in release-plz : https://github.com/release-plz/release-plz/issues/1961

I have tried to use overrides with ".git/" and ".git/*", but from what i tested, whitelisting did not work with files in subfolder of `.git` (files in .git/objects/pack/ wasn't whitlisted).


### What are the steps to reproduce the behavior?

iteratewith a ignoreWalk on a folder with a git repository, a gitignore with a line *.idx, and hidden set to false (to process .git folder) and see than idx files in `.git/objects/pack/` are filtered.

I have encounter the problem with the usage of release-plz. cf issue : https://github.com/release-plz/release-plz/issues/1961

### What is the actual behavior?

release-plz update in a repository than have *.idx in .gitignore

### What is the expected behavior?

I expect than the files of .git folder wasn't affected by filter of .gitignore
Or than, If I add `.git/*` in overrides, all files in .git and subfolders be whitelisted.

---

_Comment by @BurntSushi on 2025-01-07 20:37_

You might need `.git/**.

I'm unsure of whether gitignore should apply to `.git`. It doesn't make sense in the context of git itself, but ripgrep isn't git and its implementation is separate from git. I kind of feel like it's more unexpected for gitignore to apply to everything except for `.git`.

Also, please provide an MRE. That would be a Rust program using `ignore` on a defined set of inputs that others can reproduce.

---

_Comment by @gwen-lg on 2025-01-07 21:01_

It's to work with `.git/**`, thank you.
I had seen than `/**` exist in the code, but not understanding than it can help in this case.

I'm unsure too for a specific management of `.git` in ignore, but I think both can have arguments.
Perhaps with a specific option on WalkBuilder ? (like require_git)

---

_Comment by @BurntSushi on 2025-01-07 21:08_

`WalkBuilder` has too many options already. I'm not really a "add a knob for ever conceivable use case" kinda person, and would rather folks paper over behaviors they don't like with whitelist globs.

---

_Closed by @BurntSushi on 2025-01-07 21:08_

---

_Comment by @gwen-lg on 2025-01-07 21:12_

This make sense.
Perhaps add some example in doc of `OverrideBuilder`, could help to better see the usage and functionalities.

---

_Comment by @gwen-lg on 2025-01-07 22:13_

Sorry, I looked too quickly, the option with '.git/**' as override doesn't work, all content of `.git` folder are copied, but regular files are not copied anymore (README.md).

I don't understand why `matched` in Override return `Match::Ignore` if `mat.is_none() && self.num_whitelists() > 0 && !is_dir`.
I was expected than, if there is no match in Override, so others matches (like from .gitignore) would be tested.

---

_Comment by @BurntSushi on 2025-01-07 22:15_

I need an MRE to help your further, please.

---

_Comment by @gwen-lg on 2025-01-07 23:09_

Yes, you should fine a simple test with this repository : https://github.com/gwen-lg/ignore_override_test
The file1 is not copied contrary to what I expect (and assert is triggered).

It's enough to help you ?

---

_Comment by @gwen-lg on 2025-01-12 16:18_

@BurntSushi : Have you see my comment with a link to a test repository ?
And can you reopen the issue ?
Thank you

---

_Reopened by @BurntSushi on 2025-01-12 16:19_

---

_Comment by @BurntSushi on 2025-01-12 16:19_

I'll reopen it but it may take me some time to look into this

---

_Comment by @shulaoda on 2025-02-03 07:32_

Same issue in https://github.com/oxc-project/oxc/issues/8842

Small repo: https://github.com/shulaoda/repo-cases/tree/ignore-override-builder

```css
.
├── src
│   ├── main.rs
│   └── tests
│       ├── main.js
│       └── ignore
│           └── main.js
```

```rust
fn main() {
    let cwd = env::current_dir().unwrap().join("src");
    let mut walker = ignore::WalkBuilder::new(&cwd);

    let mut builder = OverrideBuilder::new(&cwd);

    builder.add("tests/ignore/**").unwrap();
    builder.add("!tests/**").unwrap();

    walker.overrides(builder.build().unwrap());

    let walker = walker.build();
    
    for item in walker {
        println!("{:?}", item);
    }
}
```

Result:

```rust
Ok(DirEntry { dent: Walkdir(DirEntry("<cwd>/src")), err: None })
Ok(DirEntry { dent: Walkdir(DirEntry("<cwd>/src/tests")), err: None })
```
If remove `builder.add("tests/ignore/**").unwrap();`, the result:

```rust
Ok(DirEntry { dent: Walkdir(DirEntry("<cwd>/src")), err: None })
Ok(DirEntry { dent: Walkdir(DirEntry("<cwd>/src/main.rs")), err: None })
Ok(DirEntry { dent: Walkdir(DirEntry("<cwd>/src/tests")), err: None })
```



---

_Comment by @BurntSushi on 2025-09-23 01:25_

I don't think `ignore` should be treating `.git` specially here.

---

_Closed by @BurntSushi on 2025-09-23 01:25_

---

_Label `wontfix` added by @BurntSushi on 2025-09-23 01:25_

---
