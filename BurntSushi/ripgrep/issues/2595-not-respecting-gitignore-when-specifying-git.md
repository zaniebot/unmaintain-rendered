```yaml
number: 2595
title: "not respecting `.gitignore` when specifying git ignored path ?"
type: issue
state: closed
author: f-moya
labels:
  - wontfix
assignees: []
created_at: 2023-08-24T18:11:46Z
updated_at: 2023-08-24T18:41:28Z
url: https://github.com/BurntSushi/ripgrep/issues/2595
synced_at: 2026-01-12T16:13:24Z
```

# not respecting `.gitignore` when specifying git ignored path ?

---

_@f-moya_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

`brew install ripgrep`

#### What operating system are you using ripgrep on?

macOS Ventura 13.5.1

#### Describe your bug.

Running searches on a git ignored folder shows results.

#### What are the steps to reproduce the behavior?
```sh
#/bin/bash
mkdir git_ignore_issue
cd git_ignore_issue
git init
touch .gitignore
echo "dir" >> .gitignore
echo "A" >> A.txt
git add .
git commit -m "Initial commit"
mkdir dir
echo "here" >> dir/here.txt
echo "Searching with rg"
rg here dir
echo "Searching with git grep"
git grep here dir
```

#### What is the actual behavior?
It's finding an occurrence of `here` on a file ignored by git.
command
```sh
$ rg --debug here dir
```
output
```sh
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(here)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
dir/here.txt
1:here
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```
#### What is the expected behavior?
It's not clear to me, but I believe it shouldn't find any occurrence of "here".

#### What do you think ripgrep should have done?
Work similar to how `git grep` does, _I think_.

---

_Renamed from "don't respecting `.gitignore` when specifying git ignored path ?" to "not respecting `.gitignore` when specifying git ignored path ?" by @f-moya on 2023-08-24 18:12_

---

_Comment by @BurntSushi on 2023-08-24 18:23_

You *explicitly* asked ripgrep to search `dir`. Even if `dir` is ignored, ripgrep will respect your request because doing otherwise is bonkers IMO.

`git grep` has perhaps different UX expectations here. `git grep` doesn't behave (by default) by respecting your `.gitignore`. It behaves by _only searching the files that are in your repository_. That is, the files emitted by `git ls-files`. ripgrep behaves differently. Instead, it doesn't look at what's in your repository, but rather, what is ignored by `.gitignore` (and `.ignore` and `.rgignore`). For example, `git` will let you add a gitignored-file to your repository and then `git grep` will search it even though it's still technically gitignored because the file is in the repository. But ripgrep will ignore that it's in the repository and still respect the `.gitignore` rules.

This was a design decision I made in the initial design of ripgrep. Namely, it avoids needing to read git repository state, and also permits it to treat `.gitignore` just like it treats `.ignore` and `.rgignore`. ripgrep is, after all, not a git-specific tool. It is designed to be able to search any directory tree, regardless of whether there are git repositories in it. This is unlike `git grep` which is primarily intended to only search a single git repository. (Although there are flags you can pass to it to force it to search other things.) This gives ripgrep a uniform way in how it behaves.

---

_Closed by @BurntSushi on 2023-08-24 18:23_

---

_Label `wontfix` added by @BurntSushi on 2023-08-24 18:23_

---

_Comment by @f-moya on 2023-08-24 18:32_

@BurntSushi Thanks for the detailed answer, appreciated!. Sounds reasonable. I actually encountered this using `WalkBuilder::new("/git_ignore_path")`, I couldn't find any flag to set or any setting to indicate `WalkBuilder` not to descend if it's a git ignored directory, there isn't any right ?. Thanks.

---

_Comment by @BurntSushi on 2023-08-24 18:40_

Nope.

---

_Comment by @f-moya on 2023-08-24 18:41_

Thank you!.

---
