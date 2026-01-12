```yaml
number: 1611
title: "ripgrep doesn't apply .gitignore when following a symlink to a subdirectory of a git repo"
type: issue
state: closed
author: tavianator
labels:
  - wontfix
assignees: []
created_at: 2020-06-09T19:30:04Z
updated_at: 2020-06-09T20:37:13Z
url: https://github.com/BurntSushi/ripgrep/issues/1611
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep doesn't apply .gitignore when following a symlink to a subdirectory of a git repo

---

_@tavianator_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

```
# pacman -S ripgrep
```

#### What operating system are you using ripgrep on?

Arch Linux.

#### Describe your bug.

I'm not sure if it should be expected to, but if ripgrep follows a symlink to a `git/repo/subdirectory`, it doesn't apply any of the rules from `git/repo/.gitignore`.

#### What are the steps to reproduce the behavior?

```
$ mkdir repo
$ cd repo
$ git init
Initialized empty Git repository in /home/tavianator/foo/repo/.git/
$ echo '*.o' >.gitignore
$ mkdir dir
$ echo hello >dir/not_ignored.o
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore

nothing added to commit but untracked files present (use "git add" to track)
$ cd ..
$ ln -s repo/dir link
$ rg -L .
link/not_ignored.o
1:hello
```

#### What is the actual behavior?

```
$ rg --debug -L .
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUGlink/not_ignored.o
1:hello
|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./repo/.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./repo/.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./repo/dir/not_ignored.o: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./repo/.gitignore"), original: "*.o", actual: "**/*.o", is_whitelist: false, is_only_dir: false })))
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

#### What is the expected behavior?

Maybe when ripgrep descends into a symlink to a directory, it should re-check the chain of parents to see if it is inside a git repo.


---

_Comment by @BurntSushi on 2020-06-09 19:40_

This is correct behavior. For the purposes of gitignore filtering, symlinks are filtered on the basis of the link, not on the thing it points to. Your link exists outside the git repository, so there is no expectation that `.gitignore` files inside that repo will apply to that link itself. If you need this kind of filtering, you'll need to add `.ignore` or `.rgignore` files in appropriate places.

---

_Closed by @BurntSushi on 2020-06-09 19:40_

---

_Label `wontfix` added by @BurntSushi on 2020-06-09 19:40_

---

_Comment by @tavianator on 2020-06-09 20:14_

I'm not expecting the link to be filtered, rather the things underneath that link.  If you did `cd link && rg .`, the file would get ignored.  So I thought maybe `rg -L .` should also ignore it.

---

_Comment by @BurntSushi on 2020-06-09 20:37_

Yeah, I see what you mean, but everything I said still pretty much applies. Even if I agreed that this was a bug, fixing it wouldn't be feasible. It would require resolving the symlink to its real absolute path and doing some crazy shenanigans to figure out how to correctly apply any pertinent `.gitignore` rules. The code is so complicated already, that fixing this just isn't something I'm capable of doing.

---
