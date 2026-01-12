```yaml
number: 2044
title: "Detect Vim config files as 'vim' file type"
type: pull_request
state: merged
author: rhysd
labels: []
assignees: []
merged: true
base: master
head: vim-configs
created_at: 2021-10-27T08:03:28Z
updated_at: 2021-10-27T14:59:44Z
url: https://github.com/BurntSushi/ripgrep/pull/2044
synced_at: 2026-01-12T18:23:14Z
```

# Detect Vim config files as 'vim' file type

---

_@rhysd_

`.vimrc`, `vimrc`, `_vimrc`, `.gvimrc`, `gvimrc`, `_gvimrc` are file names for Vim configurations and written in Vim script.

This PR adds them for `vim` file type detection.

Documents:

- vimrc config: https://github.com/vim/vim/blob/2446ec9b567ce2b72bd06d121f200f40bbdc8a84/runtime/doc/starting.txt#L789-L795
- gvimrc config: https://github.com/vim/vim/blob/2446ec9b567ce2b72bd06d121f200f40bbdc8a84/runtime/doc/gui.txt#L97-L102

---

_@BurntSushi requested changes on 2021-10-27 12:28_

Thanks! LGTM, but could you please ensure your changes are formatted in a way that is consistent with the surrounding code? In particular, please wrap your code to 79 columns (inclusive).

---

_Comment by @rhysd on 2021-10-27 13:55_

My text editor automatically applies `rustfmt`, but let me ensure it is formatted.

---

_Comment by @rhysd on 2021-10-27 13:57_

Ah, ok, it is marked as `#[rustfmt::skip]`. Let me fix it.

---

_Comment by @rhysd on 2021-10-27 14:02_

@BurntSushi I fixed all lines which ~excludes~ exceed the limit at 39eabf0.

---

_@BurntSushi approved on 2021-10-27 14:13_

Thanks!

---

_Merged by @BurntSushi on 2021-10-27 14:59_

---

_Closed by @BurntSushi on 2021-10-27 14:59_

---
