```yaml
number: 1115
title: how to search java file only 
type: issue
state: closed
author: northleafup
labels: []
assignees: []
created_at: 2018-11-20T02:51:04Z
updated_at: 2018-11-20T03:07:56Z
url: https://github.com/BurntSushi/ripgrep/issues/1115
synced_at: 2026-01-12T16:13:22Z
```

# how to search java file only 

---

_@northleafup_

I use the latest version `ripgrep 0.10.0` 
if I run the command ,there is java and jsp file types 
```shell
➜  ~ rg --type-list | grep java
java: *.java, *.jsp
```
how to search java file only ?

thanks  

---

_Comment by @BurntSushi on 2018-11-20 03:06_

This should be answered in the documentation. Please consider reading it before filling issues. Specifically, see: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-file-types and https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-globs

---

_Closed by @BurntSushi on 2018-11-20 03:06_

---
