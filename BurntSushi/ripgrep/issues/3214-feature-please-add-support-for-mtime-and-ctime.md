```yaml
number: 3214
title: "Feature: please add support for -mtime and -ctime similar to those from find"
type: issue
state: open
author: adrimt
labels:
  - enhancement
assignees: []
created_at: 2025-11-01T13:21:28Z
updated_at: 2025-11-01T13:30:34Z
url: https://github.com/BurntSushi/ripgrep/issues/3214
synced_at: 2026-01-12T16:13:25Z
```

# Feature: please add support for -mtime and -ctime similar to those from find

---

_@adrimt_

RIPGREP is already a very useful tool for replacing find and grep, however I encountered some cases when mtime or ctime options where needed.
Please add support for -mtime, -ctime or some time related options similar to those on find.



---

_Renamed from "please add support for -mtime -ctime similar to find" to "Please add support for -mtime and -ctime similar to find" by @adrimt on 2025-11-01 13:23_

---

_Renamed from "Please add support for -mtime and -ctime similar to find" to "Please add support for -mtime and -ctime similar to those from find" by @adrimt on 2025-11-01 13:23_

---

_Renamed from "Please add support for -mtime and -ctime similar to those from find" to "Feature: please add support for -mtime and -ctime similar to those from find" by @adrimt on 2025-11-01 13:24_

---

_Comment by @BurntSushi on 2025-11-01 13:30_

I've been thinking about doing this lately. We can use `jiff` to accept pretty fleixlbe inputs. IDK if I'll call them `-mtime` or `-ctime` though.

---

_Label `enhancement` added by @BurntSushi on 2025-11-01 13:30_

---
