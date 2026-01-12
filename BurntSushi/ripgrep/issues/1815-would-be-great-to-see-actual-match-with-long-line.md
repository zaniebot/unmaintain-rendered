```yaml
number: 1815
title: Would be great to see actual match with long-line previews
type: issue
state: closed
author: garyo
labels:
  - duplicate
assignees: []
created_at: 2021-03-08T17:56:26Z
updated_at: 2021-03-08T18:04:30Z
url: https://github.com/BurntSushi/ripgrep/issues/1815
synced_at: 2026-01-12T16:13:24Z
```

# Would be great to see actual match with long-line previews

---

_@garyo_

I have some files with very long lines (minimized Javascript, so the entire 1MB file is only one line) and rg shows that there are matches in those lines. I'm using `rg --max-columns-preview --max-columns=100` to see previews of the matching lines, which helps a bit. But since the current behavior is to show the first N characters of the line, the actual text containing the match is rarely visible, since the match could be anywhere in the very long line, and isn't likely to be near the beginning. I'd like to see this feature extended, perhaps with a new flag `--show-match-in-preview-with-context=M` that would skip over the characters (graphemes) preceding the match, such that at most M characters preceding the match are shown.
The behavior would be something similar to this sed script:
```
sed -n 's/.*\(.\{100\}StructureComponent.\{1,100\}\).*/\1/p' dist/js/longlines.js
```

---

_Comment by @BurntSushi on 2021-03-08 18:04_

Dupe of #1352. 

---

_Closed by @BurntSushi on 2021-03-08 18:04_

---

_Label `duplicate` added by @BurntSushi on 2021-03-08 18:04_

---
