```yaml
number: 1757
title: Only some ignores applied when searching a sub-directory
type: issue
state: closed
author: SimonSapin
labels:
  - bug
  - gitignore
assignees: []
created_at: 2020-12-08T14:41:22Z
updated_at: 2023-09-20T15:52:45Z
url: https://github.com/BurntSushi/ripgrep/issues/1757
synced_at: 2026-01-12T16:13:24Z
```

# Only some ignores applied when searching a sub-directory

---

_@SimonSapin_

#### What version of ripgrep are you using?

```
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

System package https://www.archlinux.org/packages/community/x86_64/ripgrep/

#### What operating system are you using ripgrep on?

Arch Linux

#### Describe your bug.

When giving a sub-directory of the current directory to search as a second command-line argument to ripgrep, only some patterns of `.ignore` in the current directory are applied.

#### What are the steps to reproduce the behavior?

```sh
mkdir tmp
cd tmp
mkdir rust
mkdir rust/target
echo needle > rust/source.rs
echo needle > rust/target/rustdoc-output.html
echo rust/target > .ignore
rg --files-with-matches needle
rg --files-with-matches needle rust
echo target >> .ignore
rg --files-with-matches needle rust
```

#### What is the actual behavior?

The first and third `rg` commands only print `rust/source.rs` as expected. However the second one prints both `rust/source.rs` and `rust/target/rustdoc-output.html`.

With `--debug` added, and `-j1` for more readable output:


```
$ rg --files-with-matches needle --debug -j1
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(needle)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./.ignore: Ignore(IgnoreMatch(Hidden))
rust/source.rs
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./rust/target: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.ignore"), original: "rust/target", actual: "rust/target", is_whitelist: false, is_only_dir: false })))

```
```
$ rg --files-with-matches needle rust --debug -j1
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(needle)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rust/source.rs
rust/target/rustdoc-output.html
```

After `echo target >> .ignore`:

```
$ rg --files-with-matches needle rust --debug -j1
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(needle)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 1 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rust/source.rs
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring rust/target: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/tmp/tmp/.ignore"), original: "target", actual: "**/target", is_whitelist: false, is_only_dir: false })))
```

#### What is the expected behavior?

Each command should only print `rust/source.rs`, as the HTML file should be ignored.

One might imagine that when searching a sub-directory only `.ignore` files under that sub-directory are considered, but the third command shows that is not the case. (The more general ignore pattern is not the desired one, it only serves to show that the `.ignore` file is indeed read.)

#### Work-around

I’ve managed to find a set up that does what I want by creating a `.ignore` file in the `rust` sub-directory of the project instead, with a `/target` pattern.

#### Other suggestion

After trying a few things and seeing unexpected results I wanted to find out details of the syntax of `.ignore` files. For example, is an initial slash meaningful?

I had to dig to find the answer in https://docs.rs/ignore/0.4.17/ignore/struct.WalkBuilder.html:

> `.ignore` files have the same semantics as `gitignore` files 

and

> `.gitignore` files have match semantics as described in the `gitignore` man page.

It would be good to repeat these two facts in ripgrep’s own docs.


---

_Label `bug` added by @BurntSushi on 2020-12-09 17:06_

---

_Comment by @goto-engineering on 2020-12-10 21:29_

This works for me with `rg 12.1.1` on macOS:
<img width="344" alt="image" src="https://user-images.githubusercontent.com/16588780/101832128-c8788800-3aeb-11eb-9163-4d57732693c4.png">

edit: nevermind, I realized that you're fixing the problem as part of the recreate steps. If I only execute the first 2 `rg` commands, I'm getting the same thing.

---

_Comment by @SimonSapin on 2020-12-10 21:54_

Ah yes, sorry, I should have separated that last step some more.

To me it’s not really a fix since it also ignore directories or files named `target` at any other location, which is not necessarily desirable. I only included it to show that `.ignore` in the current directory is read at all when searching a sub-directory. (It might make some sense if it weren’t even though that’s nat what I’d want in this case.)

---

_Label `gitignore` added by @BurntSushi on 2023-07-08 13:06_

---

_Closed by @BurntSushi on 2023-09-20 15:52_

---
