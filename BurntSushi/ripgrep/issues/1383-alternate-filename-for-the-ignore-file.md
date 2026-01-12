```yaml
number: 1383
title: Alternate filename for the ignore file
type: issue
state: closed
author: andrewdavidmackenzie
labels: []
assignees: []
created_at: 2019-09-18T23:16:42Z
updated_at: 2019-09-18T23:21:48Z
url: https://github.com/BurntSushi/ripgrep/issues/1383
synced_at: 2026-01-12T16:13:23Z
```

# Alternate filename for the ignore file

---

_@andrewdavidmackenzie_

I am looking for a recursive directory walker that uses a .gitignore compatible ignore format, but it's own filename - not .gitignore - that would live alongside .gitinore in many cases.

Do you contemplate this crate being extended to allow users to specify the name of the ignore file to be used, replacing ".gitignore".

---

_Comment by @BurntSushi on 2019-09-18 23:21_

You already can: https://docs.rs/ignore/0.4.10/ignore/struct.WalkBuilder.html#method.add_custom_ignore_filename

---

_Closed by @BurntSushi on 2019-09-18 23:21_

---
