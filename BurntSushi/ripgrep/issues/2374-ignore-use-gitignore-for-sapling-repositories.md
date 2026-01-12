```yaml
number: 2374
title: "(ignore) Use `.gitignore` for Sapling repositories"
type: issue
state: closed
author: vegerot
labels:
  - question
assignees: []
created_at: 2022-12-22T20:42:00Z
updated_at: 2022-12-29T01:46:38Z
url: https://github.com/BurntSushi/ripgrep/issues/2374
synced_at: 2026-01-12T16:13:24Z
```

# (ignore) Use `.gitignore` for Sapling repositories

---

_@vegerot_

#### Describe your feature request

[Sapling](https://sapling-scm.com/docs/introduction/) is a source control tool that is compatible with git repositories.  For example , when working on ripgrep I use `sl clone git@github.com:BurntSushi/ripgrep.git`.  Sapling puts its files (including the wrapped `.git` directory) in `<repo>/.sl`.  However, since the `ignore` crate only ignores `.gitignore` files when it detects a `.git` directory (#1229), using `rg` inside a Sapling repo ends up returning a bunch of unwanted results.

### Possible solutions
1. ripgrep patched to respect `.gitignore` files when `.git` _or_ `.sl` directories are detected
2. I could make an alias for `rg --ignore-file .gitignore`
  - won't work for nested gitignores, or when I start a search in a subdirectory
  - Will have to do the same for `fd` and other tools using `ignore`
3. Trick `ignore` into thinking I'm using regular git, maybe by making an empty `.git` directory
4. ~~Add flag `--treat-as-git` to tell ripgrep to use `.gitignore` files~~
  - How do you specify where the root of the repo is?
  - @BurntSushi mentioned in #2364 that `--no-require-git` is this
 

I prefer solution 1.  Would you accept a patch that checks for `.git` or `.sl` when deciding if you're in a git repo?

---

_Comment by @BurntSushi on 2022-12-23 01:46_

This was discussed a bit here: https://github.com/BurntSushi/ripgrep/pull/2364

Namely, I talk about why your (1) idea is processor not a good idea right now, and also suggest a work around that I don't see in your list of alternatives.

---

_Label `question` added by @BurntSushi on 2022-12-23 01:47_

---

_Comment by @vegerot on 2022-12-23 22:57_

Thank you 😊 

`--no-require-git` sounds good 👍.  This was an idea I wrote about but didn't know about `--no-require-git`.  Will `--no-require-git` search for parent `.gitignore` files until my home/root directory?

---

_Comment by @vegerot on 2022-12-29 01:46_

`--no-require-git` is perfect!  Thank you 🙏 
Happy New Year 🎆 🎈 🎊 

---

_Closed by @vegerot on 2022-12-29 01:46_

---
