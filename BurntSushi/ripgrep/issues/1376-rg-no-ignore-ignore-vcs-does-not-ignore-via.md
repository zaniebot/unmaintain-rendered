```yaml
number: 1376
title: "\"rg --no-ignore --ignore-vcs\" does not ignore via .gitignore"
type: issue
state: closed
author: blueyed
labels:
  - enhancement
  - rollup
  - gitignore
assignees: []
created_at: 2019-09-12T12:53:26Z
updated_at: 2023-11-25T20:04:03Z
url: https://github.com/BurntSushi/ripgrep/issues/1376
synced_at: 2026-01-12T16:13:23Z
```

# "rg --no-ignore --ignore-vcs" does not ignore via .gitignore

---

_@blueyed_

While looking into https://github.com/BurntSushi/ripgrep/issues/1374 I've noticed that `rg --no-config --no-ignore --ignore-vcs --debug` does not appear to use a `.gitignore` file (from the current directory):

```
% rg --no-config --no-ignore --ignore-vcs --debug luarocks
DEBUG|rg::args|src/args.rs:544: not reading config files because --no-config is present
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:59: literal prefixes detected: Literals { lits: [Complete(luarocks)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|ignore/src/walk.rs:1639: ignoring ./.SRCINFO: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore/src/walk.rs:1639: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore/src/walk.rs:1639: ignoring ./.gitignore: Ignore(IgnoreMatch(PKGBUILD
13:makedepends=('luarocks')
15:source=("https://luarocks.org/$_rockname-$pkgver-$_rockrel.src.rock")
19:  luarocks --tree="$pkgdir/usr" install --deps-mode=none "$_rockname-$pkgver-$_rockrel.src.rock"
Hidden))
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

`.gitignore` contains just "*", i.e. everything should be ignored.

It works when using `--ignore` additionally, but `--ignore-vcs` should be enough to enable this, no?

It also does not work with all `--ignore-*` options:

> rg --no-config --no-ignore --ignore-vcs --ignore-global --ignore-dot --ignore-messages --ignore-parent …

#### What version of ripgrep are you using?

ripgrep 11.0.2

---

_Comment by @BurntSushi on 2019-09-12 13:05_

This is working as intended as far as I can see. The only way to override `--no-ignore` is with `--ignore`. If you add `--no-ignore`, then you can't selectively turn other things on. We could add logic that checks whether `--ignore-vcs` is set (I think), and then use its presence to override more general options. (And then to the same for other ignore flags.) The logic is already pretty hairy though.

---

_Label `enhancement` added by @BurntSushi on 2019-09-12 13:05_

---

_Comment by @blueyed on 2019-09-12 13:07_

Ah, I see.
Maybe it would help to add debug output for handling those args?
(I've expected `--no-ignore` to turn on all flags, and that `--ignore-vcs` would turn off the vcs flag again)

---

_Comment by @blueyed on 2019-09-12 13:08_

I.e. `--no-ignore` could be expanded internally into having specified all `--no-ignore` flags (except for `--no-ignore-messages` probably).

---

_Comment by @BurntSushi on 2019-09-12 13:11_

I don't have that kind of arbitrary control over arg parsing. Even with my suggested fix, `--no-ignore --ignore-vcs --no-ignore` wouldn't work like one would expect, since the second `--no-ignore` won't actually override `--ignore-vcs`.

---

_Label `gitignore` added by @BurntSushi on 2020-05-08 11:31_

---

_Label `rollup` added by @BurntSushi on 2023-11-25 19:30_

---

_Closed by @BurntSushi on 2023-11-25 20:04_

---
