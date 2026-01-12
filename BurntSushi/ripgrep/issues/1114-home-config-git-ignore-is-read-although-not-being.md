```yaml
number: 1114
title: $HOME/.config/git/ignore is read although not being used
type: issue
state: closed
author: blueyed
labels:
  - question
assignees: []
created_at: 2018-11-19T17:36:03Z
updated_at: 2018-12-21T00:56:40Z
url: https://github.com/BurntSushi/ripgrep/issues/1114
synced_at: 2026-01-12T16:13:22Z
```

# $HOME/.config/git/ignore is read although not being used

---

_@blueyed_

I've just noticed that ripgrep reads global ignore configuration from `~/.config/git/ignore`, but then does not use it?!

Given t2.pyc (which is not a binary file) in /tmp/t:

```
% strace -f -e file rg --debug -l .
getcwd("/tmp/t", 512)                   = 7
openat(AT_FDCWD, "/home/user/.gitconfig", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/home/user/.config/git/config", O_RDONLY|O_CLOEXEC) = 3
stat("/home/user/.config/git/ignore", {st_mode=S_IFREG|0644, st_size=6, ...}) = 0
openat(AT_FDCWD, "/home/user/.config/git/ignore", O_RDONLY|O_CLOEXEC) = 3
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
stat("./", {st_mode=S_IFDIR|0755, st_size=100, ...}) = 0
stat("./", {st_mode=S_IFDIR|0755, st_size=100, ...}) = 0
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
getcwd("/tmp/t", 512)                   = 7
…
t2.pyc
```

`~/.config/git/ignore` contains just "*.pyc".

I wonder where the "8 extensions" afterwards is coming from?  Are those defaults?

It looks like `~/.config/git/ignore` only gets applied for paths in `~`?!

If this is intended loading the config could be skipped in this case then?

For what it's worth, I've added `--ignore-file=…` to my wrapper script to use `~/.ignore` always.

(I was confused about "permission denied" errors for `*.pyc` file when searching in `/usr/lib/python3.7` - those appear to happen when ripgrep tests them for being binary files)


---

_Comment by @BurntSushi on 2018-11-19 18:58_

gitignores are only respected inside git repositories:

```
$ cat ~/.config/git/ignore
*.pyc

$ touch foo.pyc foo.py

$ rg --files
foo.py
foo.pyc

$ git init
Initialized empty Git repository in /tmp/ripgrep-1114/.git/

$ rg --files
foo.py
```

> I wonder where the "8 extensions" afterwards is coming from? Are those defaults?

Possibly? I don't know. They don't appear in my debug output:

```
$ rg --files --debug
DEBUG|rg::config|src/config.rs:37: /home/andrew/.ripgreprc: arguments loaded from config file: ["--max-columns=500", "--colors=match:bg:0xff,0x7f,0x00", "--colors=match:fg:white", "--colors=line:none", "--colors=line:fg:magenta", "--colors=path:fg:green", "--type-add=got:*_test.go"]
DEBUG|rg::args|src/args.rs:508: final argv: ["rg", "--max-columns=500", "--colors=match:bg:0xff,0x7f,0x00", "--colors=match:fg:white", "--colors=line:none", "--colors=line:fg:magenta", "--colors=path:fg:green", "--type-add=got:*_test.go", "--files", "--debug"]
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./foo.pyc: Ignore(IgnoreMatch(Gitignore(Globfoo.py
 { from: Some("/home/andrew/.config/git/ignore"), original: "*.pyc", actual: "**/*.pyc", is_whitelist: false, is_only_dir: false })))
```

---

_Label `question` added by @BurntSushi on 2018-11-19 18:58_

---

_Comment by @blueyed on 2018-12-21 00:56_

Thanks for the answer, closing.

---

_Closed by @blueyed on 2018-12-21 00:56_

---
