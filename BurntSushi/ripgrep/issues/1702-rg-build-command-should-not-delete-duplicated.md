```yaml
number: 1702
title: rg-build-command should not delete duplicated flags 
type: issue
state: closed
author: jixiuf
labels:
  - invalid
assignees: []
created_at: 2020-10-11T09:02:32Z
updated_at: 2020-10-11T11:52:01Z
url: https://github.com/BurntSushi/ripgrep/issues/1702
synced_at: 2026-01-12T16:13:24Z
```

# rg-build-command should not delete duplicated flags 

---

_@jixiuf_

#### What version of ripgrep are you using?
12.1.1

#### How did you install ripgrep?

brew install rg

#### What operating system are you using ripgrep on?

macOS 

#### Describe your bug.

I define a command ` rg-rerun-exclude-dir`  to rerun last search but exclude selected directory with flag: -g !dir1.
It works when I call it first time ,but when I call it twice, I go 
```
/usr/local/bin/rg --color=always --colors=match:fg:red --colors=path:fg:magenta --colors=line:fg:green --colors=column:none -n --column --type-add=gyp\:\*.gyp --type-add=gyp\:\*.gypi -z --pcre2 --type=all -g !foo !bar --no-heading --no-config --type=elisp -e \(\?\<\!\[a-zA-Z0-9_-\]\)delete-dups\(\?\!\[a-zA-Z0-9-_\]\)

!bar: No such file or directory (os error 2)

```
I think the `-g !foo !bar` in command  should be ` -g !foo -g !bar` 


```
(defun rg-rerun-exclude-dir ()
  "Rerun last search but exclude selected diredctory with flag: -g !dir"
  (interactive)
  (let ((flags (rg-search-flags rg-cur-search)))
    (setq flags (append flags `("-g" ,(concat "!" (read-string "exclude  : ")))))
    (print flags)
    (setf (rg-search-flags rg-cur-search) flags)
    (rg-rerun)))

  (define-key rg-mode-map (kbd "x") ' rg-rerun-exclude-dir)

```


What do you think ripgrep should have done?

Maybe `delete-dups` should not be called in `rg-build-command`.


---

_Comment by @jixiuf on 2020-10-11 09:21_

feel free to close , I find  another way(` --glob=!name` instead of `-g !name` )  to implements this 
```


(defun rg-rerun-exclude ()
  "Rerun last search but exclude selected filename or diredctory with flag: --glob='!*name*'"
  (interactive)
  (let ((flags (rg-search-flags rg-cur-search))
        (dir (read-string "exclude(file or dir): ")))
    (setq flags (append flags (list (format "--glob='!*%s*'"  dir))))
    (setf (rg-search-flags rg-cur-search) flags)
    (rg-rerun)))
```

---

_Comment by @BurntSushi on 2020-10-11 11:51_

This looks like a problem in your lisp program. Not ripgrep. I'm not sure why you've filed a bug against ripgrep. The error message you got from ripgrep is correct.

---

_Closed by @BurntSushi on 2020-10-11 11:51_

---

_Label `invalid` added by @BurntSushi on 2020-10-11 11:52_

---
