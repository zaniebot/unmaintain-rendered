```yaml
number: 3141
title: Add Qt types for resource files and ui declaration
type: pull_request
state: closed
author: cmaureir
labels:
  - rollup
assignees: []
base: master
head: add_qt_extensions
created_at: 2025-09-07T11:26:18Z
updated_at: 2025-09-08T09:53:19Z
url: https://github.com/BurntSushi/ripgrep/pull/3141
synced_at: 2026-01-12T18:23:15Z
```

# Add Qt types for resource files and ui declaration

---

_@cmaureir_

qrc are the resource files for data related to user interfaces, and ui is the extension that the Qt Designer generates, for Widget based projects.

---

_Comment by @cmaureir on 2025-09-07 11:26_

Two references if needed:
* https://doc.qt.io/qt-6/resources.html
* https://doc.qt.io/qt-6/uic.html

---

_Label `rollup` added by @BurntSushi on 2025-09-07 15:09_

---

_Comment by @BurntSushi on 2025-09-07 15:09_

Thanks! The `ui` name is way too general for something like this, so I've used `qui` instead.

---

_Comment by @cmaureir on 2025-09-08 09:53_

Cool, thanks!

---

_Closed by @cmaureir on 2025-09-08 09:53_

---
