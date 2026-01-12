```yaml
number: 1364
title: output file path have garbled if file coding is not utf-8
type: issue
state: closed
author: pengpengxp
labels:
  - invalid
assignees: []
created_at: 2019-09-03T09:17:30Z
updated_at: 2019-09-03T11:04:10Z
url: https://github.com/BurntSushi/ripgrep/issues/1364
synced_at: 2026-01-12T16:13:23Z
```

# output file path have garbled if file coding is not utf-8

---

_@pengpengxp_

I run ripgrep 11.0.2 (rev 3de31f7527) on windows.

If I run rg with -H --heading. My output file path have garbled if the file path have chinese path.

I find it is because the output file path encoding is always utf-8.

I known I can use -E/--encoding the set the encoding. But it not work on the output file path.

Is there any argument to specify output file path encoding？

---

_Comment by @BurntSushi on 2019-09-03 09:30_

This issue is not actionable. Please re-open another issue and follow the template. It's there for a reason.

(File paths in Windows are not UTF-8. They are UCS-2. ripgrep passes them through as-is.)

---

_Closed by @BurntSushi on 2019-09-03 09:30_

---

_Label `invalid` added by @BurntSushi on 2019-09-03 11:04_

---
