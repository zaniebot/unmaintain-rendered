```yaml
number: 2542
title: Default preprocessor configuration by file type
type: issue
state: closed
author: rosejn
labels: []
assignees: []
created_at: 2023-06-22T02:41:45Z
updated_at: 2023-06-22T11:18:13Z
url: https://github.com/BurntSushi/ripgrep/issues/2542
synced_at: 2026-01-12T16:13:24Z
```

# Default preprocessor configuration by file type

---

_@rosejn_

#### Describe your feature request

Since ripgrep already has support for preprocessor scripts, it would be great if we could specify a set of preprocessor scripts and/or commands by filetype in the config file.  For example we could use pdftotext for pdfs like you show in the documentation, but also nbdime for ipython notebooks, even OCR for images, etc.

Thanks for a great tool!

---

_Comment by @BurntSushi on 2023-06-22 11:18_

You should do this in the preprocessor script itself. I don't see any reason why it needs to be baked into ripgrep.

---

_Closed by @BurntSushi on 2023-06-22 11:18_

---
