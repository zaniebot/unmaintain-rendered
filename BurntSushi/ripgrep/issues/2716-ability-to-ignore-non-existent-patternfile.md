```yaml
number: 2716
title: "Ability to ignore non-existent `PATTERNFILE`"
type: issue
state: closed
author: 5HT2
labels:
  - wontfix
assignees: []
created_at: 2024-01-19T06:04:04Z
updated_at: 2024-01-22T03:16:27Z
url: https://github.com/BurntSushi/ripgrep/issues/2716
synced_at: 2026-01-12T16:13:24Z
```

# Ability to ignore non-existent `PATTERNFILE`

---

_@5HT2_

#### Describe your feature request

I'd like a flag to be able to suppress a no such file error when using `-f` (`--file`), for use in scripts where something like a regex filter of the output is optional.

Of course, I can always have the script just make a file containing a newline to suppress this error, but then I'm left with a garbage unwanted file alongside the script, it'd be much cleaner to have a `--ignore-empty-file` / `--file-ignore-empty` (struggling to think of a naming scheme that wouldn't be confused with `--ignore-file`, maybe `--file-optional`?).

In my opinion, having to add an if case to scripts which checks for a file not existing instead of simply having the option to optionally take a file would be much cleaner and preferable, especially given that currently, the file can just be empty and it will default to not filtering anything!

I understand that not giving an error by default if someone typo's the name of the file they'd like to filter with leaves you with confusing behavior which doesn't return an output if you don't know what happened, which is why I'm suggesting this as a separate flag in place of `-f`, or potentially one that would be used in combination with `-f`.

I'm more than happy to implement this myself so long as this is a desirable feature, and suggestions for an arg name would be appreciated :p

---

_Comment by @BurntSushi on 2024-01-19 12:19_

Sorry, but I do not believe this use case is worth a new flag.

---

_Closed by @BurntSushi on 2024-01-19 12:19_

---

_Label `wontfix` added by @BurntSushi on 2024-01-19 12:19_

---

_Comment by @5HT2 on 2024-01-22 03:16_

That's fair! Thank you for time and incredibly prompt response 🤍

---
