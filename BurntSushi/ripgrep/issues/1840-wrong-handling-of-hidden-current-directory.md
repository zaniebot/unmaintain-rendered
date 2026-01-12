```yaml
number: 1840
title: "Wrong handling of \"hidden\" current directory"
type: issue
state: closed
author: lbolla
labels:
  - invalid
assignees: []
created_at: 2021-04-01T12:38:37Z
updated_at: 2021-04-01T13:06:01Z
url: https://github.com/BurntSushi/ripgrep/issues/1840
synced_at: 2026-01-12T16:13:24Z
```

# Wrong handling of "hidden" current directory

---

_@lbolla_

#### What version of ripgrep are you using?

12.1.1

#### How did you install ripgrep?

Github binary release

#### What operating system are you using ripgrep on?

Ubuntu 20.04

#### Describe your bug.

When the current directory is "hidden" (name starts with dot), `rg` fails to find matches in subdirectories.

#### What are the steps to reproduce the behavior?

Consider this code:
```
# rg finds matches in this case.
 » rg flycheck-pycheckers-ena .emacs.d
.emacs.d/elpa/flycheck-pycheckers-20200828.1814/flycheck-pycheckers.el
206:(define-obsolete-variable-alias 'flycheck-pycheckers-enabled-codes 'flycheck-pycheckers-enable-codes "")
207:(flycheck-def-option-var flycheck-pycheckers-enable-codes
260:             (eval (when flycheck-pycheckers-enable-codes
261:                     (concat "--enable-codes=" (when (listp flycheck-pycheckers-enable-codes)
262:                                                 (mapconcat 'identity flycheck-pycheckers-enable-codes ",")))))

# Now I cd into a "hidden" directory and rerun the search
~ » cd .emacs.d

# No matches found!
~/.emacs.d(master*) » rg flycheck-pycheckers-ena

# No matter if I specify to search the current directory with "."
~/.emacs.d(master*) » rg flycheck-pycheckers-ena .

# But it's fine if I specify the subdirectory
~/.emacs.d(master*) » rg flycheck-pycheckers-ena elpa
elpa/flycheck-pycheckers-20200828.1814/flycheck-pycheckers.el
206:(define-obsolete-variable-alias 'flycheck-pycheckers-enabled-codes 'flycheck-pycheckers-enable-codes "")
207:(flycheck-def-option-var flycheck-pycheckers-enable-codes
260:             (eval (when flycheck-pycheckers-enable-codes
261:                     (concat "--enable-codes=" (when (listp flycheck-pycheckers-enable-codes)
262:                                                 (mapconcat 'identity flycheck-pycheckers-enable-codes ",")))))
```

---

_Comment by @BurntSushi on 2021-04-01 12:45_

Please follow the instructions in the issue template and include the output when running with the `--debug` flag.

---

_Comment by @lbolla on 2021-04-01 12:53_

@BurntSushi Ah, I must have missed mention to the `--debug` flag! Thanks.

Running with `rg --debug` gave me a hint of what's going on. The "hidden" directory (".emacs.d") is actually a symlink to a subdirectory inside a repository: `rg` is picking up the parent's ".gitignore". Running `rg --no-ignore-parent` works.

Thanks again for your help.

---

_Closed by @lbolla on 2021-04-01 12:53_

---

_Label `invalid` added by @BurntSushi on 2021-04-01 13:06_

---
