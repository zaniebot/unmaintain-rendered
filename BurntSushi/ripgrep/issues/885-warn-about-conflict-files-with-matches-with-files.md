```yaml
number: 885
title: Warn about conflict --files-with-matches with --files-without-match
type: issue
state: closed
author: kenorb
labels:
  - invalid
assignees: []
created_at: 2018-04-15T00:28:53Z
updated_at: 2018-04-15T00:55:52Z
url: https://github.com/BurntSushi/ripgrep/issues/885
synced_at: 2026-01-12T16:13:22Z
```

# Warn about conflict --files-with-matches with --files-without-match

---

_@kenorb_

#### What version of ripgrep are you using?

`ripgrep 0.8.1`

#### What operating system are you using ripgrep on?

macOS High Sierra

#### Feature

Currently this command works fine:

    rg --files-with-matches --files-without-match string1

but I think it will never produce any output, since both params are contradictory.

---

_Comment by @BurntSushi on 2018-04-15 00:55_

This is working as intended, and is consistent with the documentation for these flags:

```
    -l, --files-with-matches
            Only print the paths with at least one match.
            This overrides --files-without-match.
        --files-without-match
            Only print the paths that contain zero matches. This inverts/negates the
            --files-with-matches flag.
            This overrides --files-with-matches.
```

---

_Closed by @BurntSushi on 2018-04-15 00:55_

---

_Label `invalid` added by @BurntSushi on 2018-04-15 00:55_

---
