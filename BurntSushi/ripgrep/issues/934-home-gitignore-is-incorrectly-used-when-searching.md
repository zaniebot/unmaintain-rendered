```yaml
number: 934
title: $HOME/.gitignore is incorrectly used when searching in other directories such as /tmp
type: issue
state: closed
author: joshunger
labels:
  - bug
assignees: []
created_at: 2018-06-02T15:23:57Z
updated_at: 2018-11-27T20:58:34Z
url: https://github.com/BurntSushi/ripgrep/issues/934
synced_at: 2026-01-12T16:13:22Z
```

# $HOME/.gitignore is incorrectly used when searching in other directories such as /tmp

---

_@joshunger_

#### What version of ripgrep are you using?
```
ripgrep 0.8.1
-SIMD -AVX
```

#### How did you install ripgrep?

```
 brew install ripgrep
```

#### What operating system are you using ripgrep on?
mac 10.13.4

#### Describe your question, feature request, or bug.

Create a `.gitignore` in your `$HOME` directory.  Cd over to `/tmp`, mkdir a pattern excluded in `.gitignore` and execute `rg`.

#### If this is a bug, what are the steps to reproduce the behavior?

```
echo "*.apk" > ~/.gitignore
cd /tmp
touch foo.apk
rg --debug --files
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Repeat {
    e: Literal {
        chars: [
            'z'
        ],
        casei: false
    },
    r: Range {
        min: 0,
        max: Some(
            0
        )
    },
    greedy: true
}
DEBUG/globset/globset/src/lib.rs:401: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.keystone_install_lock: Ignore(IgnoreMatch(Hidden))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./foo.apk: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/junger/.gitignore"), original: "*.apk", actual: "**/*.apk", is_whitelist: false, is_only_dir: false })))
```

#### If this is a bug, what is the actual behavior?

See above.

#### If this is a bug, what is the expected behavior?
ripgrep should not use .gitignore in your $HOME directory when searching in other directories such as `/tmp`


---

_Renamed from ".gitignore is used in $HOME when searching in other directorys such as /tmp" to "$HOME/.gitignore is not ignored when searching in other directorys such as /tmp" by @joshunger on 2018-06-02 15:24_

---

_Renamed from "$HOME/.gitignore is not ignored when searching in other directorys such as /tmp" to "$HOME/.gitignore is incorrectly used when searching in other directorys such as /tmp" by @joshunger on 2018-06-02 15:27_

---

_Renamed from "$HOME/.gitignore is incorrectly used when searching in other directorys such as /tmp" to "$HOME/.gitignore is incorrectly used when searching in other directories such as /tmp" by @joshunger on 2018-06-02 15:30_

---

_Comment by @BurntSushi on 2018-06-02 15:31_

Hmm, I can't seem to reproduce this issue on master:

```
$ pwd
/tmp/ripgrep-934
$ echo foo > $HOME/.gitignore
$ cat $HOME/.gitignore
foo
$ touch foo bar
$ rg --files
bar
foo
```

---

_Comment by @joshunger on 2018-06-02 15:37_

How do I install master?

```
➜  /tmp mkdir ripgrep-934
➜  /tmp cd ripgrep-934 
➜  ripgrep-934 echo foo > $HOME/.gitignore
➜  ripgrep-934 cat $HOME/.gitignore
foo
➜  ripgrep-934 touch foo bar
➜  ripgrep-934 rg --files
bar
➜  ripgrep-934 
```

---

_Comment by @BurntSushi on 2018-06-02 15:37_

I can't reproduce this in `0.8.1` either.

Do you have any global git configuration set? For example, if you have `core.excludesFile` set in your `~/.gitconfig` to `$HOME/.gitignore`, then that might be the cause. Indeed, if I add

```
[core]
  excludesFile = /home/andrew/.gitignore
```

to my `$HOME/.gitconfig`, then I can reproduce the problem:

```
$ rg --files
bar
```

where my `--debug` output is:

```
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Repeat {
    e: Literal {
        chars: [
            'z'
        ],
        casei: false
    },
    r: Range {
        min: 0,
        max: Some(
            0
        )
    },
    greedy: true
}
DEBUG/globset/globset/src/lib.rs:401: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./foo: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/andrew/.gitignore"), original: "foo", actual: "**/foo", is_whitelist: false, is_only_dir: false })))
```

This does actually seem like a bug to me. I don't think ripgrep should be applying global gitignore rules outside of a git repo.

---

_Comment by @BurntSushi on 2018-06-02 15:38_

> How do I install master?

The README has build instructions. But given my previous comment, I don't think it's necessary, assuming the issue here is indeed your global git config.

---

_Comment by @joshunger on 2018-06-02 15:40_

@BurntSushi awesome catch!

```
➜  ripgrep-934 git config --global -l       
hub.host=...
hub.protocol=http
hub.host=...
hub.host=...
hub.host=...
hub.host=...
core.excludesfile=~/.gitignore
➜  ripgrep-934 
```

I also have a ton of `hub.host` but that can be ignored. 🤔 

I also don't recalled why this is set in `excludesfile`.

---

_Label `bug` added by @BurntSushi on 2018-06-02 15:41_

---

_Comment by @roblourens on 2018-06-02 18:16_

> I don't think ripgrep should be applying global gitignore rules outside of a git repo.

Even inside a git repo, this might be confusing for the same reasons that led me to use `--no-ignore-parent` in vscode. If it still uses global gitignore rules in git repos, it would be nice to be able to disable that.

---

_Closed by @BurntSushi on 2018-07-22 14:45_

---

_Comment by @BurntSushi on 2018-07-22 14:45_

This bug is fixed on master, and @roblourens I've added a new `--no-ignore-global` flag that should do what you want!

---

_Comment by @joshunger on 2018-07-22 22:03_

Thanks @BurntSushi !

---

_Comment by @roblourens on 2018-07-23 01:18_

Thanks, reopening the vscode issue to use it.

---

_Comment by @sgraham on 2018-11-27 20:58_

In case anyone else lands here and was using this as a "feature" (with apologies for the xkcd.com/1172/):

I had been using this bug to add a local `.gitignore` at the root of a tree that isn't itself a git repo to ignore additional things inside child git repos. Renaming `.gitignore` to `.rgignore` restores the previous behaviour.

---
