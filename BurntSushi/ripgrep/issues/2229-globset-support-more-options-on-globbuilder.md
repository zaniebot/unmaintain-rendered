```yaml
number: 2229
title: "[globset] support more options on GlobBuilder?"
type: issue
state: open
author: WindSoilder
labels:
  - enhancement
  - help wanted
  - question
assignees: []
created_at: 2022-06-09T02:25:04Z
updated_at: 2022-06-14T12:26:36Z
url: https://github.com/BurntSushi/ripgrep/issues/2229
synced_at: 2026-01-12T16:13:24Z
```

# [globset] support more options on GlobBuilder?

---

_@WindSoilder_

#### Describe your feature request

Refer to [GlobBuilder](https://docs.rs/globset/latest/globset/struct.GlobBuilder.html), it supports [case_insensitive](https://docs.rs/globset/latest/globset/struct.GlobBuilder.html#method.case_insensitive), [literal_separator](https://docs.rs/globset/latest/globset/struct.GlobBuilder.html#method.literal_separator) and [backslash_escape](https://docs.rs/globset/latest/globset/struct.GlobBuilder.html#method.backslash_escape).

I wonder if it can support more options includes:
```rust
/// Whether or not paths that contain components that start with a `.`
/// will require that `.` appears literally in the pattern; `*`, `?`, `**`,
/// or `[...]` will not match. This is useful because such files are
/// conventionally considered hidden on Unix systems and it might be
/// desirable to skip them when listing files.
require_literal_leading_dot: bool

/// if given pattern contains `**`, this flag check if `**` matches hidden directory.
/// For example: if true, `**` will match `.abcdef/ghi`.
recursive_match_hidden_dir: bool
```

Which mainly works for hidden directory or hidden file

---

_Comment by @BurntSushi on 2022-06-09 13:43_

I'm sure it can be supported. I didn't originally because I wasn't convinced of their utility. And more to the point, it somewhat encourages writing non-portable code. Namely, while a leading dot is a convention used on Unix to indicate a hidden file or directory, this convention is less used on Windows. On Windows, whether something is hidden or not is actually a first class property of the file's metadata.

On the other hand, globs _tend_ to be written by end users, and in that context, portability isn't always needed.

In any case, it would be good to explain why you want these things. What would you use them for if they existed?

---

_Label `question` added by @BurntSushi on 2022-06-09 13:43_

---

_Comment by @WindSoilder on 2022-06-14 03:10_

Background: I'm trying to help to make `ls` command better in [nushell](https://www.nushell.sh/book/moving_around.html), it uses [glob](https://github.com/rust-lang/glob) crate.

But it seems that `glob` doesn't support non-UTF8 path, and it would be tricky to make `glob` support it.  I'm searching for other replacement, one choice is the `globset` crate

If we use `ls -a **`, we would expect `recursive_match_hidden_dir` to be true and return hidden directories.  Simply use `ls **` will not return hidden directories (only for unix)


---

_Comment by @BurntSushi on 2022-06-14 12:26_

If you can find a way to add the things you need along with tests without any major reworking of the code, I'd accept a patch.

---

_Label `enhancement` added by @BurntSushi on 2022-06-14 12:26_

---

_Label `help wanted` added by @BurntSushi on 2022-06-14 12:26_

---
