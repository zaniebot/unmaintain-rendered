```yaml
number: 1598
title: ".gitignore? ignored!"
type: issue
state: closed
author: tifrel
labels:
  - doc
assignees: []
created_at: 2020-05-27T08:51:27Z
updated_at: 2023-11-25T19:16:47Z
url: https://github.com/BurntSushi/ripgrep/issues/1598
synced_at: 2026-01-12T16:13:23Z
```

# .gitignore? ignored!

---

_@tifrel_

#### ripgrep version

    ❯ rg --version
    ripgrep 12.1.0
    -SIMD -AVX (compiled)
    +SIMD -AVX (runtime)

#### Means of installation

    sudo pacman -S ripgrep

#### OS

    ❯ uname -r
    5.6.12-1-MANJARO

#### Bug description

`ripgrep` searches through files that are listed in `.gitignore`, possibly related to closed [issue 1336](https://github.com/BurntSushi/ripgrep/issues/1336)

#### Reproduction

    ❯ ls -A
    bakfile  .gitignore  testfile
    ❯ cat bakfile
    Find me
    ❯ cat testfile
    Find me
    ❯ cat .gitignore
    bakfile
    ❯ rg 'Find me'
    testfile
    1:Find me

    bakfile     # <<< should not be searched
    1:Find me
    ❯ rg 'Find me' --glob='!bakfile'
    testfile
    1:Find me
    ❯ type rg    # check if rg is aliased
    rg is /usr/bin/rg
    ❯ rg --debug 'Find me'
    DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(Find me)], limit_size: 250, limit_class: 10 }
    DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
    DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
    DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
    DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
    DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
    DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
    testfile
    1:Find me

    bakfile
    1:Find me
    DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
    DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
    DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
    DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
    DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes


#### What is the expected behavior?

Respecting the .gitignore file, as per spec.


---

_Comment by @okdana on 2020-05-27 09:03_

Think you want this

```
--no-require-git
    By default, ripgrep will only respect global gitignore rules,
    .gitignore rules and local exclude rules if ripgrep detects that
    you are searching inside a git repository. This flag allows you to
    relax this restriction such that ripgrep will respect all git
    related ignore rules regardless of whether you're searching in a
    git repository or not.

    This flag can be disabled with --require-git.
```

(See https://github.com/BurntSushi/ripgrep/commit/d0659282e65dde46e0b5f373d89042b2fe1d4884)

---

_Comment by @tifrel on 2020-05-27 09:29_

I already figured out to just rename my `.gitignore` to `.ignore`. However I wish to encourage to update the documentation (might do a PR on that), because in the [corresponding section](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#automatic-filtering) the claim to respect the `.gitignore` is literally the first point on the list and I can't find the line where it states that you need to be in a git repo for this to be supported. It tells you how to turn it off, not how to turn it on.

If doing the PR, do I assume correctly that the precedence is `.rgignore` > `.ignore` > `.gitignore`?

---

_Comment by @okdana on 2020-05-27 09:50_

I think that's right, assuming that list is highest to lowest priority. And it only applies where there's a conflict (like one says `foo` and one says `!foo`); otherwise it just merges them all together

---

_Comment by @tifrel on 2020-05-27 12:27_

There you go maintainers: https://github.com/BurntSushi/ripgrep/pull/1600

---

_Label `doc` added by @BurntSushi on 2020-05-28 11:54_

---

_Comment by @BurntSushi on 2023-11-25 19:16_

It looks like this was fixed by https://github.com/BurntSushi/ripgrep/pull/1600.

---

_Closed by @BurntSushi on 2023-11-25 19:16_

---
