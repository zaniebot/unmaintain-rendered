```yaml
number: 264
title: another case when russian text not matched
type: issue
state: closed
author: acidnik
labels:
  - duplicate
assignees: []
created_at: 2016-12-01T11:46:12Z
updated_at: 2016-12-01T12:27:55Z
url: https://github.com/BurntSushi/ripgrep/issues/264
synced_at: 2026-01-12T16:13:21Z
```

# another case when russian text not matched

---

_@acidnik_

follow up to #251 
```
$ cat test.txt
слово другое
слова другие
слову другому
$ grep 'слов.*друг' test.txt
слово другое
слова другие
слову другому
$ rg 'слов.*друг' test.txt
$ rg --version
ripgrep 0.3.1
```

---

_Comment by @BurntSushi on 2016-12-01 12:15_

This appears to be a duplicate of #251 and was therefore fixed in 0473df1ef5721143941fb7f883e22b17292b35bb. I can confirm it's working fine on current master:

```
$ cat test
слово другое
слова другие
слову другому
$ grep 'слов.*друг' test
слово другое
слова другие
слову другому
$ rg 'слов.*друг' test
1:слово другое
2:слова другие
3:слову другому
```

I'll try to get a new release out in the next few days.

---

_Closed by @BurntSushi on 2016-12-01 12:15_

---

_Label `duplicate` added by @BurntSushi on 2016-12-01 12:15_

---

_Comment by @acidnik on 2016-12-01 12:27_

Sorry, I thougth 251 already released

---
