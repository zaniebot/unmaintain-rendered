```yaml
number: 1063
title: Restrict files from command line
type: issue
state: closed
author: Nokel81
labels:
  - invalid
assignees: []
created_at: 2018-09-24T15:36:25Z
updated_at: 2018-09-24T15:46:18Z
url: https://github.com/BurntSushi/ripgrep/issues/1063
synced_at: 2026-01-12T16:13:22Z
```

# Restrict files from command line

---

_@Nokel81_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

sudo apt-get install ripgrep

#### What operating system are you using ripgrep on?

Distributor ID:	Ubuntu
Description:	Ubuntu 18.04.1 LTS
Release:		18.04
Codename:	bionic

#### Describe your question, feature request, or bug.

*Feature Request:* Restricting files from the command line.

This would allow for per command basis of limiting which files were searched based on their names. The pattern would be another regex and it would be a positive (or negative) match based on which of two parameters were passed in.

---

_Label `invalid` added by @BurntSushi on 2018-09-24 15:46_

---

_Comment by @BurntSushi on 2018-09-24 15:46_

Please consider reading the [GUIDE](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md) before filing feature requests. I'll add this tip to the issue template.

In particular, see the section on [glob rules](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-globs) and [file types](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-file-types).

> sudo apt-get install ripgrep

ripgrep is [not available in Bionic](https://packages.ubuntu.com/search?keywords=ripgrep&searchon=names&suite=all&section=all). Could you please clarify how you installed it?

---

_Closed by @BurntSushi on 2018-09-24 15:46_

---
