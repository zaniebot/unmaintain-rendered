```yaml
number: 672
title: "gitignore: Disregard plain wildcard"
type: issue
state: closed
author: zachriggle
labels: []
assignees: []
created_at: 2017-11-10T19:42:06Z
updated_at: 2017-11-10T20:05:56Z
url: https://github.com/BurntSushi/ripgrep/issues/672
synced_at: 2026-01-12T16:13:22Z
```

# gitignore: Disregard plain wildcard

---

_@zachriggle_

It is occasionally useful to have Git ignore *all* files unless explicitly added to the index via `git add --force`.  This would appear in a gitignore file like:

```
# Ignore everything not tracked
*
```

For example, this is generally useful when an entire home directory is a git repository (for dotfiles, etc. ~/bin/, etc) without needing to add every single random directory to `.gitignore`.

In this case, `rg` will not search anything unless given the `--unrestricted` flag.  This is correct in the strict sense, but cumbersome -- and also means that it will ignore any other rules it discovers in the "global" gitignore (`git config --global core.excludesfile ~/.gitignore_global`)

I'd like to suggest that if the specific pattern `*` is discovered in a `.gitignore`, that the rule is disregarded.

---

_Comment by @BurntSushi on 2017-11-10 19:47_

No, sorry. ripgrep isn't going to start ignoring patterns arbitrarily.

With that said, I agree it is cumbersome. The `-u` flag or specifying explicit file paths (e.g., `rg foo *`) should both work though. Another work around is to override your `.gitignore` by whitelisting everything in your `.ignore`. e.g., `echo '!*' > $HOME/.ignore` or similar.

---

_Closed by @BurntSushi on 2017-11-10 19:47_

---

_Comment by @zachriggle on 2017-11-10 19:58_

Would you accept a pull request for a `--no-ignore-wildcard` flag which does this conditionally? 

---

_Comment by @BurntSushi on 2017-11-10 20:05_

No.

---
