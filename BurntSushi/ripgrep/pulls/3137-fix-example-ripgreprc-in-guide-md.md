```yaml
number: 3137
title: fix example .ripgreprc in GUIDE.md
type: pull_request
state: closed
author: turkishmaid
labels: []
assignees: []
base: master
head: patch-1
created_at: 2025-09-01T20:01:34Z
updated_at: 2025-09-01T21:46:28Z
url: https://github.com/BurntSushi/ripgrep/pull/3137
synced_at: 2026-01-12T18:23:15Z
```

# fix example .ripgreprc in GUIDE.md

---

_@turkishmaid_

`!.git/*` does not do what the example suggests, i.e. include general hidden files and directories, _except_ `.git/` folder contents. Matches inside `.git/` folders will instead be returned. Without the `*` it works as expected. Verified with `zsh`.

---

_Comment by @BurntSushi on 2025-09-01 20:06_

This change doesn't make any sense.

---

_Closed by @BurntSushi on 2025-09-01 20:06_

---

_Comment by @turkishmaid on 2025-09-01 20:47_

@BurntSushi – Well, I thought the same, but see this transcript, verbatim from my console:


```
% export -p | grep RIPGREP_CONFIG_PATH
export RIPGREP_CONFIG_PATH=/Users/sara/.config/ripgrep/rc



% cat $RIPGREP_CONFIG_PATH
# Don't let ripgrep vomit really long lines to my terminal, and show a preview.
--max-columns=150
--max-columns-preview

# Search hidden files / directories (e.g. dotfiles) by default
--hidden
--glob=!.git/*

# who cares about case!?
--smart-case



% rg remote
the_stow/.git/hooks/push-to-checkout.sample
12:# of the remote repository has any difference from the currently

the_stow/.git/hooks/pre-push.sample
4:# push" after it has checked the remote status, but before anything has been
9:# $1 -- Name of the remote to which the push is being done
12:# If pushing without using a named remote those arguments will be equal.
17:#   <local ref> <local oid> <remote ref> <remote oid>
22:remote="$1"
27:while read local_ref local_oid remote_ref remote_oid
34:		if test "$remote_oid" = "$zero"
40:			range="$remote_oid..$local_oid"

the_stow/.git/hooks/update.sample
23:#   This boolean sets whether remotely creating branches will be denied
110:	refs/remotes/*,commit)
113:	refs/remotes/*,delete)

the_stow/root/subdir/.stowed3
3:remotes



% vi $RIPGREP_CONFIG_PATH



% cat $RIPGREP_CONFIG_PATH
# Don't let ripgrep vomit really long lines to my terminal, and show a preview.
--max-columns=150
--max-columns-preview

# Search hidden files / directories (e.g. dotfiles) by default
--hidden
--glob=!.git/

# who cares about case!?
--smart-case



% rg remote               
the_stow/root/subdir/.stowed3
3:remotes
```

---

_Comment by @BurntSushi on 2025-09-01 21:46_

This example is just demonstrating the syntax of the config file.

The only thing that is said here about `--glob=!.git/*` or `--glob\n!.git/*` is the comment in the example itself:

```
# Using glob patterns to include/exclude files or folders 
```

Yet your characterization of what the example is showing is:

> i.e. include general hidden files and directories, except .git/ folder contents.

Which is not at all what is actually said.

Hence this change makes no sense.

---
