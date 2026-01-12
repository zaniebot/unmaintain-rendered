```yaml
number: 399
title: _rg.ps1 not included in windows zips
type: issue
state: closed
author: dstcruz
labels:
  - bug
assignees: []
created_at: 2017-03-09T12:00:49Z
updated_at: 2017-03-12T20:50:08Z
url: https://github.com/BurntSushi/ripgrep/issues/399
synced_at: 2026-01-12T16:13:21Z
```

# _rg.ps1 not included in windows zips

---

_@dstcruz_

Is there a reason why we are not including `_rg.ps1` in the windows zip files for release? I can't find the file in the repo, so I assume that this is a generated file, which makes it hard to include in the [chocolatey windows package](https://chocolatey.org/packages/ripgrep), since it is not something I can easily download.

---

_Comment by @BurntSushi on 2017-03-09 12:04_

I don't think there's any specific reason. My Windows shell skills are pretty bad, so I think it just needs to be added to the release script here: https://github.com/BurntSushi/ripgrep/blob/master/appveyor.yml#L42

---

_Label `bug` added by @BurntSushi on 2017-03-09 12:04_

---

_Comment by @BurntSushi on 2017-03-12 20:50_

fixed in https://github.com/BurntSushi/ripgrep/pull/401

---

_Closed by @BurntSushi on 2017-03-12 20:50_

---
