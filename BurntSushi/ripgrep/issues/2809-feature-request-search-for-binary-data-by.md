```yaml
number: 2809
title: "Feature Request: search for binary data by supplying a hex string"
type: issue
state: closed
author: RecRanger
labels:
  - invalid
assignees: []
created_at: 2024-05-16T03:10:54Z
updated_at: 2024-05-27T12:16:12Z
url: https://github.com/BurntSushi/ripgrep/issues/2809
synced_at: 2026-01-12T16:13:24Z
```

# Feature Request: search for binary data by supplying a hex string

---

_@RecRanger_

#### Describe your feature request

I wish I could use this tool to search for binary contents within files, similar to [bgrep](https://github.com/tmbinc/bgrep). 

By supplying a `--hex` flag, followed by bytes in hex, I wish it could search the file. 

---

_Comment by @BurntSushi on 2024-05-16 12:57_

You already can:

```
$ printf 'abc\xFF\xFE\xAAxyz' | rg -ao '(?-u:\xFF\xFE\xAA)' | xxd
00000000: fffe aa0a                                ....
```

---

_Closed by @BurntSushi on 2024-05-16 12:57_

---

_Label `invalid` added by @BurntSushi on 2024-05-16 12:57_

---

_Comment by @RecRanger on 2024-05-23 16:33_

Does this support non-unicode sequences? 

---

_Comment by @BurntSushi on 2024-05-23 16:46_

Huh? My example demonstrates definitively that it does...

---

_Comment by @BurntSushi on 2024-05-23 16:46_

`\xFF\xFE` is not valid UTF-8.

---

_Comment by @RecRanger on 2024-05-24 01:46_

Awesome, thanks! Unfortunately the average Joe doesn't know how to identify a valid unicode string from hex chars. Thanks for your help :) 

---

_Comment by @RecRanger on 2024-05-26 08:50_

Can you explain why you use `-a` here? The documentation isn't very clear on what the flag actually changes:

```
  -a, --text                      Search binary files as if they were text.
```

---

_Comment by @BurntSushi on 2024-05-26 11:51_

Because you're looking at the succinct docs. Use `--help` (as `-h` says at the top) to see the expanded docs:

```
    -a, --text
        This flag instructs ripgrep to search binary files as if they were
        text. When this flag is present, ripgrep's binary file detection is
        disabled. This means that when a binary file is searched, its contents
        may be printed if there is a match. This may cause escape codes to be
        printed that alter the behavior of your terminal.

        When binary file detection is enabled, it is imperfect. In general, it
        uses a simple heuristic. If a NUL byte is seen during search, then the
        file is considered binary and searching stops (unless this flag is
        present). Alternatively, if the --binary flag is used, then ripgrep
        will only quit when it sees a NUL byte after it sees a match (or
        searches the entire file).

        This flag overrides the --binary flag.

        This flag can be disabled with --no-text.
```

The `-a/--text` flag is pretty standard among greps. It disables binary detection. By default, greps will treat `\x00` (`NUL`) as indicating that a file is binary data and maybe should be skipped or at least not printed to a terminal.

---

_Comment by @RecRanger on 2024-05-27 09:00_

> By default, greps will treat \x00 (NUL) as indicating that a file is binary data and maybe should be skipped or at least not printed to a terminal.

I still can't wrap my head around how this differs from the `--binary` flag, which also means "files should not be skipped, even if it's binary". Funny that `--text` and `--binary` have nearly identical meanings, depsite being opposites generally. 

---

_Comment by @BurntSushi on 2024-05-27 12:16_

* The default for recursive search is to completely skip binary files. That is, whenever a NUL byte is seen, ripgrep immediately stops the search and skips the rest of the file.
* When `--binary` is given, it means, "ripgrep should not omit matches in files that are binary data, but otherwise should still be careful about printing binary data to a terminal." So when a NUL byte is found, if a match hasn't been found yet, ripgrep will continue searching. If a match is found, then a message is printed and the rest of the file is skipped: `binary file matches (found "\0" byte around offset 0)`.
* When `--text` is given, it means, "ripgrep should treat all files as text files and not concern itself with binary data." In this case, there is no NUL byte detection or guarding against printing binary data to your terminal.

I probably would have chosen a different model for exposing this, but it wasn't worth abdicating the `-a/--text` flag which is widely supported.

---
