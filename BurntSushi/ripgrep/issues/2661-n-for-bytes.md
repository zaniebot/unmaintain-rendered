```yaml
number: 2661
title: "-N for bytes"
type: issue
state: closed
author: chadbrewbaker
labels:
  - wontfix
assignees: []
created_at: 2023-11-27T22:47:42Z
updated_at: 2023-11-28T00:06:15Z
url: https://github.com/BurntSushi/ripgrep/issues/2661
synced_at: 2026-01-12T16:13:24Z
```

# -N for bytes

---

_@chadbrewbaker_

#### Describe your feature request

I need something that does -N but instead of lines I want N bytes before and after the matched string.

Something like "-N_bytes" ? Represent bytes as base64?

```bash
N bytes before
My matched string
N bytes after
```

This would be a good integration test benchmark. It enumerates all sections that do parameterized SQLite queries.
```bash
ripgrep -N_bytes 128 "VALUES" /System/Applications/*.app/Contents/MacOS/*
```
On second thought, it might be better to have PREFIX+string+SUFFIX as one base64 per line? Even better if it does:

```bash
 FILE_NAME OFFSET_BYTES  PREFIX+string+SUFFIX 
 ```

Apparently -Hb already gives filenames and offsets?



---

_Comment by @BurntSushi on 2023-11-28 00:05_

You can use `.{N}<pattern>.{N}`. The `-r` and `-o` may also be useful, perhaps in combination with capture groups.

I don't know what base64 has to do with this.

---

_Closed by @BurntSushi on 2023-11-28 00:05_

---

_Label `wontfix` added by @BurntSushi on 2023-11-28 00:06_

---
