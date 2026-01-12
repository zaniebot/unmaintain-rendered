```yaml
number: 1110
title: Can ripgrep be used as a tool to search a file as unit instead of line?
type: issue
state: closed
author: stardiviner
labels: []
assignees: []
created_at: 2018-11-16T13:10:51Z
updated_at: 2018-11-18T08:38:46Z
url: https://github.com/BurntSushi/ripgrep/issues/1110
synced_at: 2026-01-12T16:13:22Z
```

# Can ripgrep be used as a tool to search a file as unit instead of line?

---

_@stardiviner_

I want to use ripgrep as a command tool to search file as unit instead of line.
So that I can search "Linux Emacs" in files if a file matched, show it in result.

---

_Comment by @BurntSushi on 2018-11-16 13:24_

Please consider perusing the flags before opening an issue next time. The flag you're looking for is `--files-with-matches`.

---

_Closed by @BurntSushi on 2018-11-16 13:24_

---

_Comment by @stardiviner on 2018-11-18 03:13_

Hi, @BurntSushi I created following snippet of code after your hints.
Here it is:
```emacs-lisp
(defun rg-direct (word)
  (format "rg %s" word))

(defun rg-files-with-matches-beginning (word dir)
  (format "rg --files-with-matches \"%s\" %s" word dir))

(defun rg-files-with-matches-middle (word)
  (format " | xargs rg --files-with-matches \"%s\"" word))

(defun rg-files-with-matches-end (word)
  (format " | xargs rg --heading \"%s\"" word))

(defun rg-files-construct-command (words dir)
  (case (length words)
    (1 (rg-direct (car words)))
    (2 (concat (rg-files-with-matches-beginning) (rg-files-with-matches-end)))
    (3 (concat (rg-files-with-matches-beginning (car words) dir)
               (rg-files-with-matches-middle (cadr words))
               (rg-files-with-matches-end (car (reverse words)))))
    (t (concat (rg-files-with-matches-beginning (car words) dir)
               ;; OPTIMIZE: Do I have to use (car (mapcan ...))?
               (car (mapcan
                     (lambda (word) (list (rg-files-with-matches-middle word)))
                     (delq (car (reverse words)) (cdr words))))
               (rg-files-with-matches-end (car (reverse words)))))))

;; (rg-files-construct-command
;;  (split-string "hello world stardiviner myself" " ")
;;  default-directory)

(defun rg-search-words-by-files (directory)
  "Search multiple words by file as unit instead of line.

rg --files-with-matches \"foo\" . | xargs rg --files-with-matches \"bar\" | ... | xargs rg --heading \"baz\"
"
  (interactive (list (completing-read "Dir: " `(,(expand-file-name org-directory)
                                                ,default-directory))))
  (let* ((words (split-string (read-from-minibuffer "Words: ") " "))
         (command (rg-files-construct-command words directory)))
    (compilation-start command 'rg-mode)))
```

I have some ideas about this code,
- how to improve this code to fit `rg.el` style? (I wish to use `rg.el`'s API functions like macro `define-rg-search`) I would like to hear your reviews. Thanks in advance.
- Is it possible to integrated it into `rg.el`, If yes, I would like to send PR.

---

_Comment by @BurntSushi on 2018-11-18 03:16_

I don't understand what problem you're trying to solve. I don't know what `rg.el` is.

I am happy to help, but please be considerate of others' time. Please:

1. Use a common shell scripting language, such as bash, to show the commands you're trying to run.
2. Describe, at a high level, what problem you want to solve. If you're unsure of how to do this, then complete, small, reproducible examples of what you would like to do is a good start.

If you are trying to integrate ripgrep into another tool, then you might consider speaking with practitioners of said tool instead of me.

---

_Comment by @stardiviner on 2018-11-18 08:38_

Sorry, wrong place post this comment. I supposed to post comment on rg.el issues.

---
