```yaml
number: 2475
title: Filter out files by last modified age. 
type: issue
state: closed
author: nickolay-kondratyev
labels:
  - wontfix
assignees: []
created_at: 2023-03-23T05:52:45Z
updated_at: 2023-03-23T15:57:44Z
url: https://github.com/BurntSushi/ripgrep/issues/2475
synced_at: 2026-01-12T16:13:24Z
```

# Filter out files by last modified age. 

---

_@nickolay-kondratyev_

#### Describe your feature request

Provide an option along the lines of `find -mtime` to only look at files that were modified in the last X minutes.

```
    -mtime n[smhdw]
             If no units are specified, this primary evaluates to true if the difference between the file last modification time and the time find was started, rounded up to the next full
             24-hour period, is n 24-hour periods.

             If units are specified, this primary evaluates to true if the difference between the file last modification time and the time find was started is exactly n units.  Please
             refer to the -atime primary description for information on supported time units.
```
Doesn't have to have units, can be simplified to just accept `--modified-in-last-minutes X`

The idea is to be able to look within code files that were modified in the last hour, in the last day etc...

The main use case is to be able to do something along the lines of `rg --modified-in-last-minutes X --glob '*.your-extension' | fzf` which doesnt seem to work if we try to use `find` before using `rg`


---

_Comment by @BurntSushi on 2023-03-23 10:54_

I think this is too much scope creep. And it is a feature that will beget more features. If we allow filtering by mtime, then what about creation time? And various other attributes?

I think it would be better to just use `find` and build a pipeline with ripgrep.

---

_Closed by @BurntSushi on 2023-03-23 10:54_

---

_Label `wontfix` added by @BurntSushi on 2023-03-23 10:54_

---

_Comment by @BurntSushi on 2023-03-23 10:55_

> The main use case is to be able to do something along the lines of rg --modified-in-last-minutes X --glob '*.your-extension' | fzf which doesnt seem to work if we try to use find before using rg

I don't see any reason why using `find` wouldn't work here.

---

_Comment by @nickolay-kondratyev on 2023-03-23 15:53_

@BurntSushi if you try to use `find` piped to `rg` then `.gitignore` functionality doesn't appear to work with rg

Example:
```
find . -mmin -60 -type f -print0 | xargs -0 rg . --hidden --with-filename --line-number --follow | fzf
```
goes into `.git` directory. 

---

_Comment by @BurntSushi on 2023-03-23 15:57_

Well, no, it wouldn't. Because there you're passing those files explicitly to ripgrep, and if you give a file to ripgrep explicitly, then it will always search it.

You can use something like `fd` instead if you want gitignore. I don't know if it has a last modified filter, but that's probably where it would be.

---
