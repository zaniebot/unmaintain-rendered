```yaml
number: 2860
title: "[feature request] only return file, line, and column for multiple matches"
type: issue
state: closed
author: iloveitaly
labels: []
assignees: []
created_at: 2024-07-29T14:40:02Z
updated_at: 2024-08-01T13:58:25Z
url: https://github.com/BurntSushi/ripgrep/issues/2860
synced_at: 2026-01-12T16:13:25Z
```

# [feature request] only return file, line, and column for multiple matches

---

_@iloveitaly_

#### Describe your feature request

Option to limit output to include file, line, and column only. No matching output.

The reason for this is passing the content into a tool like `fzf`. Right now, I need to `cut` the ripgrep output before passing to `ripgrep`. This works fine, but adds additional complexity to shell commands. Would be great if I could control the ripgrep output instead.

[Here's an example shell script.](https://github.com/iloveitaly/dotfiles/blob/613633c9ba274fb0b9b6c6004c8fb77d02a12c7a/.functions#L313-L322)

Thanks for the amazing tool!

---

_Comment by @BurntSushi on 2024-07-29 14:52_

You can do this today with the `-r/--replace` flag:

```
$ echo foobar | rg --no-heading --with-filename --line-number --column ob -or ''
<stdin>:1:3:
```

Otherwise I don't think it's worth adding a dedicated flag with this. I'm totally fine with needing to run `cut` on the output for cases like this.

---

_Closed by @BurntSushi on 2024-07-29 14:52_

---

_Comment by @iloveitaly on 2024-08-01 13:58_

ah, `-r` is a great workaround! Thanks

---
