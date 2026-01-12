```yaml
number: 149
title: Option to supress log/error messages
type: issue
state: closed
author: cloudhead
labels:
  - enhancement
assignees: []
created_at: 2016-10-05T14:39:26Z
updated_at: 2023-03-18T14:05:24Z
url: https://github.com/BurntSushi/ripgrep/issues/149
synced_at: 2026-01-12T16:13:21Z
```

# Option to supress log/error messages

---

_@cloudhead_

It would be nice to have an option to supress log messages and errors like in `ag`. This is useful for using ripgrep in scripts. This comes up when using ripgrep in directories where certain files are not readable.

In `ag`, there is:

```
--silent
 Suppress all log messages, including errors.
```

Since there is already a `--quiet` option, this might not be the best name for it, maybe `--no-log`?


---

_Comment by @BurntSushi on 2016-10-05 14:49_

Which log messages do you want to see suppressed?


---

_Comment by @cloudhead on 2016-10-05 14:52_

Anything that isn't results. For example, when folders aren't readable, you currently get:

```
IO error for operation on ./lost+found: Permission denied (os error 13)
```


---

_Comment by @BurntSushi on 2016-10-05 14:52_

But that's emitted to `stderr`. You could hide those messages by redirecting `stderr` to `/dev/null`. e.g., `rg foo 2> /dev/null`.


---

_Comment by @cloudhead on 2016-10-05 15:01_

Hm indeed! I guess if all non-result text is emitted on stderr, that should be a fine solution. Ripgrep isn't always used within a shell context though, but I guess there is always the possibility to control stderr.


---

_Comment by @BurntSushi on 2016-10-05 15:05_

I think GNU find has the same behavior. I don't see an option to suppress these types of messages, but if it did, I'd consider that a stronger precedent than `ag` personally.


---

_Comment by @cloudhead on 2016-10-05 15:10_

`grep` has the following:

```
   -s, --no-messages
              Suppress error messages about nonexistent or unreadable files.
```

However, I do agree that it seems redundant if messages are properly sent to stderr.


---

_Comment by @cloudhead on 2016-10-05 15:12_

I'm guessing this exists because sending stderr to /dev/null is not always appropriate or possible, but can't think of any specific examples. My use-case is solved with redirection.


---

_Comment by @BurntSushi on 2016-10-05 15:20_

@cloudhead I consider `grep` a very strong precedent for a feature like this, although the short `-s` option is already taken. I'd be fine adding a `--no-messages` option.


---

_Label `enhancement` added by @BurntSushi on 2016-10-05 15:20_

---

_Comment by @cloudhead on 2016-10-05 15:22_

Great. I don't think a short option is necessary.


---

_Closed by @BurntSushi on 2016-11-06 19:36_

---

_Comment by @ElectricRCAircraftGuy on 2021-12-08 20:02_

> But that's emitted to `stderr`. You could hide those messages by redirecting `stderr` to `/dev/null`. e.g., `rg foo 2> /dev/null`.

Perfect! Full example:

```bash
rg 'my regex search' 'path/to/search' 2> /dev/null
```

---

_Comment by @thistehneisen on 2023-03-18 14:05_

Sorry for bringing back old topic, but I don't seem to find an option to suppress messages like:
```
WARNING: stopped searching binary file after match (found "\0" byte around offset 687940)
WARNING: stopped searching binary file after match (found "\0" byte around offset 881429)
```
Neither `2>` works, nor `--no-messages`

---
