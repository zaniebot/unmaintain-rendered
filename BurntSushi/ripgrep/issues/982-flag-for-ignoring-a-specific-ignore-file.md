```yaml
number: 982
title: Flag for ignoring a specific ignore file
type: issue
state: closed
author: sauyon
labels:
  - wontfix
assignees: []
created_at: 2018-07-16T19:03:20Z
updated_at: 2018-07-16T20:20:42Z
url: https://github.com/BurntSushi/ripgrep/issues/982
synced_at: 2026-01-12T16:13:22Z
```

# Flag for ignoring a specific ignore file

---

_@sauyon_

#### What version of ripgrep are you using?

ripgrep 0.8.1 -SIMD -AVX

#### How did you install ripgrep?

Nix

#### What operating system are you using ripgrep on?

Mac OS 10.13.5

#### Describe your question, feature request, or bug.

I have a dotfiles git repository that I clone to machines to share config which has a gitignore that excludes everything. This is a bit annoying in terms of `rg` because if I want to search for things in my home directory I have to either give up the gitignore feature or temporarily move my .gitignore.

I would like a flag to ignore a specific .gitignore (or .ignore) file.

---

_Comment by @BurntSushi on 2018-07-16 19:57_

> I have to either give up the gitignore feature or temporarily move my .gitignore.

I'm not convinced these are your only two options. For example, you can add a `.ignore` right along side your `.gitignore` that overrides any rules you might have in the `.gitignore`.

For what it's worth, I have the same setup (dot files in a git repo), and I just set that repo's `status.showUntrackedFiles` to `no`, and then I don't need a `.gitignore` at all.

---

_Label `question` added by @BurntSushi on 2018-07-16 19:58_

---

_Comment by @sauyon on 2018-07-16 20:06_

The `showUntrackedFiles` is probably the way to go :P.

FWIW putting a .ignore file with `!*` doesn't quite solve my problem though because it unignores things (like .git) that I would prefer to be kept ignored.

---

_Comment by @BurntSushi on 2018-07-16 20:20_

@sauyon Yes, you need to get fancier than that perhaps, e.g., `![!.]*`.

Either way, I don't think this feature is a good fit for ripgrep. I'd rather see people solve the problem of "what to ignore" using established mechanisms.

---

_Closed by @BurntSushi on 2018-07-16 20:20_

---

_Label `wontfix` added by @BurntSushi on 2018-07-16 20:20_

---

_Label `question` removed by @BurntSushi on 2018-07-16 20:20_

---
