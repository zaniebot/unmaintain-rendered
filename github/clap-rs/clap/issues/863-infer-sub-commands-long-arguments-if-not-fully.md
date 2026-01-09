---
number: 863
title: Infer sub-commands/long-arguments if not fully typed/supplied but are fully unambiguous
type: issue
state: closed
author: neandrake
labels:
  - A-parsing
assignees: []
created_at: 2017-02-19T18:11:32Z
updated_at: 2018-08-02T03:30:02Z
url: https://github.com/clap-rs/clap/issues/863
synced_at: 2026-01-07T13:12:19-06:00
---

# Infer sub-commands/long-arguments if not fully typed/supplied but are fully unambiguous

---

_Issue opened by @neandrake on 2017-02-19 18:11_

If a sub command or long argument is only partially entered, it would be an ergonomic benefit if clap could infer the right intended sub command or long argument as long as it's unambiguous.

This is currently possible by manually defining aliases for all possible partial matches, but is tedious and would be easy to make mistakes if there are many similar commands.

This is a feature I use heavily with Mercurial, here are some examples.

```lang=console
$ hg bookmark
  [snipped output]

$ hg book
  [snipped output]

$ hg bo
  [snipped output]

$ hg b
hg: command 'b' is ambiguous:
    backout bisect blame bookmarks branch branches bundle
```
```lang=console
$ hg push
  [snipped output]

$ hg pus
  [snipped output]

$ hg pu
hg: command 'pu' is ambiguous:
    pull purge push
```

```lang=console
$ hg stat --change master^
  [snipped output]

$ hg stat --cha master^
  [snipped output]

$ hg stat --ch master^
  [snipped output]

$ hg stat --c master^
hg status: option --c not a unique prefix
hg status [OPTION]... [FILE]...

show changed files in the working directory

options ([+] can be repeated):

 -A --all                 show status of all files
 -m --modified            show only modified files
 -a --added               show only added files
 -r --removed             show only removed files
 -d --deleted             show only deleted (but tracked) files
 -c --clean               show only files without changes
 -u --unknown             show only unknown (not tracked) files
 -i --ignored             show only ignored files
 -n --no-status           hide status prefix
 -C --copies              show source of copied files
 -0 --print0              end filenames with NUL, for use with xargs
    --rev REV [+]         show difference from revision
    --change REV          list the changed files of a revision
 -I --include PATTERN [+] include names matching the given patterns
 -X --exclude PATTERN [+] exclude names matching the given patterns
 -S --subrepos            recurse into subrepositories
    --mq                  operate on patch repository

(use 'hg status -h' to show more help)
```

---

_Comment by @kbknapp on 2017-02-19 18:40_

I like it! Thanks for filing. I'll have to play around with how exactly I want to add this or how it will interact with the `suggestions` feature because I don't want misspelled words to get inferred, only shortened unambiguous words.

---

_Label `C: parsing` added by @kbknapp on 2017-02-19 19:46_

---

_Label `D: intermediate` added by @kbknapp on 2017-02-19 19:46_

---

_Label `P3: want to have` added by @kbknapp on 2017-02-19 19:46_

---

_Label `T: new feature` added by @kbknapp on 2017-02-19 19:46_

---

_Label `W: 2.x` added by @kbknapp on 2017-02-19 19:46_

---

_Added to milestone `2.21.0` by @kbknapp on 2017-02-22 21:43_

---

_Closed by @homu on 2017-03-11 18:31_

---
