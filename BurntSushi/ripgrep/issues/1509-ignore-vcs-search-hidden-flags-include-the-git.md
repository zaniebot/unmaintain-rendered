```yaml
number: 1509
title: "\"Ignore VCS\" + \"search hidden\" flags include the .git directory"
type: issue
state: closed
author: foundryspatial-duncan
labels:
  - wontfix
assignees: []
created_at: 2020-03-06T19:57:12Z
updated_at: 2025-12-16T03:39:35Z
url: https://github.com/BurntSushi/ripgrep/issues/1509
synced_at: 2026-01-12T16:13:23Z
```

# "Ignore VCS" + "search hidden" flags include the .git directory

---

_@foundryspatial-duncan_

#### What version of ripgrep are you using?

ripgrep 11.0.2 (rev 3de31f7527)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

from apt

#### What operating system are you using ripgrep on?

Ubuntu 18.04

#### Describe your question, feature request, or bug.

This seems like it could be a bug. If it's the intended behavior, it's kind of non-intuitive.
Searching with `--hidden` and `--ignore-vcs` includes the `.git/` folder in results.
Similar to issue #807, which looks like it _was_ a bug.

#### If this is a bug, what are the steps to reproduce the behavior?

In a git repo (ie, a folder with a `.git/` subfolder):

* If you run `rg --files`, it will list the files as expected: no hidden files, nothing from .gitignore, and no `.git/` directory since "ignore VCS" is enabled by default.

* If you run `rg --files --ignore-vcs`, it's the same as above, again because "ignore VCS" is enabled by default.

* if you add the "hidden flag" and run: `rg --files --hidden --ignore-vcs`, it will include the contents of the hidden `.git/` directory, even though it's one of the paths that should still be ignored by the "ignore VCS" option.

#### If this is a bug, what is the actual behavior?

`--hidden` takes precedence over `--ignore-vcs` and the search includes `.git/`

Output from an example directory: `rg --hidden --files --ignore-vcs --debug`

```
DEBUG|rg::config|src/config.rs:40: /home/duncan/.ripgreprc: arguments loaded from config file: []
DEBUG|globset|globset/src/lib.rs:430: glob converted to regex: Glob { glob: "**/*~", re: "(?-u)^(?:/?|.*/)[^/]*\\~$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('~')]) }
DEBUG|globset|globset/src/lib.rs:430: glob converted to regex: Glob { glob: "**/._*", re: "(?-u)^(?:/?|.*/)\\._[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('_'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:430: glob converted to regex: Glob { glob: "**/.smbdelete*", re: "(?-u)^(?:/?|.*/)\\.smbdelete[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('s'), Literal('m'), Literal('b'), Literal('d'), Literal('e'), Literal('l'), Literal('e'), Literal('t'), Literal('e'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:430: glob converted to regex: Glob { glob: "**/.v8flags.*", re: "(?-u)^(?:/?|.*/)\\.v8flags\\.[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('v'), Literal('8'), Literal('f'), Literal('l'), Literal('a'), Literal('g'), Literal('s'), Literal('.'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 5 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 4 regexes
hello.txt
foobar.js
.git/config
.git/HEAD
.git/description
.git/hooks/post-update.sample
.git/hooks/update.sample
.git/hooks/commit-msg.sample
.git/hooks/pre-push.sample
.git/hooks/pre-applypatch.sample
.git/hooks/pre-commit.sample
.git/hooks/applypatch-msg.sample
.git/hooks/pre-receive.sample
.git/hooks/fsmonitor-watchman.sample
.git/hooks/prepare-commit-msg.sample
.git/hooks/pre-rebase.sample
.git/info/exclude
```

(I created a simple git repo with a couple files and commented everything out of my .ripgreprc file)

#### If this is a bug, what is the expected behavior?

`.git/` and other VCS-related things should still not be searched unless `--no-ignore-vcs` is explicitly set. 

While the `.git/` dir isn't technically something from `.gitignore`, it's still ignored as part of the "ignore VCS" flag, so I'd expect it to still be ignored, regardless of the settings for hidden files.


---

_Comment by @BurntSushi on 2020-03-06 20:02_

This is expected behavior. The bug you linked was a particular matching bug. As you note, `.git` is not ignored by a .gitignore. it is ignored only because ripgrep skips hidden files and directories by default. Passing `--hidden` tells ripgrep to search those. The `--no-ignore-vcs` flag only applies to gitignore related rules. There is otherwise no baked in special paths that are ignored.

If you want your command to still skip .git directories, then adding it to a `.ignore` file is the correct solution.

---

_Closed by @BurntSushi on 2020-03-06 20:02_

---

_Label `invalid` added by @BurntSushi on 2020-03-06 20:02_

---

_Label `invalid` removed by @BurntSushi on 2020-03-06 20:02_

---

_Label `wontfix` added by @BurntSushi on 2020-03-06 20:02_

---

_Comment by @foundryspatial-duncan on 2020-03-06 21:22_

Ah! Thanks for clarifying. I had assumed there was some internal blacklist of VCS things to ignore if that option was set. It totally makes sense if it was only being ignored because it's a hidden folder. 

---
