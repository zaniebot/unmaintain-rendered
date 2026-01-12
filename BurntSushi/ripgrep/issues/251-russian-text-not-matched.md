```yaml
number: 251
title: russian text not matched
type: issue
state: closed
author: acidnik
labels:
  - bug
assignees: []
created_at: 2016-11-25T13:36:29Z
updated_at: 2016-11-28T23:32:38Z
url: https://github.com/BurntSushi/ripgrep/issues/251
synced_at: 2026-01-12T18:23:11Z
```

# russian text not matched

---

_@acidnik_

```
$ cat test.txt 
привет
Привет
ПрИвЕт
$ rg -i привет test.txt 
$ grep -i привет test.txt 
привет
Привет
ПрИвЕт
$ locale
LANG=en_US.UTF-8
LANGUAGE=
LC_CTYPE="en_US.utf8"
LC_NUMERIC="en_US.utf8"
LC_TIME="en_US.utf8"
LC_COLLATE="en_US.utf8"
LC_MONETARY="en_US.utf8"
LC_MESSAGES="en_US.utf8"
LC_PAPER="en_US.utf8"
LC_NAME="en_US.utf8"
LC_ADDRESS="en_US.utf8"
LC_TELEPHONE="en_US.utf8"
LC_MEASUREMENT="en_US.utf8"
LC_IDENTIFICATION="en_US.utf8"
LC_ALL=en_US.utf8
$ file test.txt
test.txt: UTF-8 Unicode text
$ rg --version
ripgrep 0.3.0
```
looks like a bug to me

---

_Label `bug` added by @BurntSushi on 2016-11-25 14:53_

---

_Comment by @BurntSushi on 2016-11-25 14:56_

Hmm, yes, it does appear to be a bug. I don't know the source of it yet, but it does at least appear that that the literals detected are correct:

```
$ rg привет test.txt --debug -i
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'п',                                                                                                                                                                                         
        'р',                                                                                                                                                                                         
        'и',                                                                                                                                                                                         
        'в',                                                                                                                                                                                         
        'е',                                                                                                                                                                                         
        'т'                                                                                                                                                                                          
    ],                                                                                                                                                                                               
    casei: true                                                                                                                                                                                      
}                                                                                                                                                                                                    
DEBUG:grep::literals: required literals found: [Cut(ПРИВ), Cut(пРИВ), Cut(ПрИВ), Cut(прИВ), Cut(ПРиВ), Cut(пРиВ), Cut(ПриВ), Cut(приВ), Cut(ПРИв), Cut(пРИв), Cut(ПрИв), Cut(прИв), Cut(ПРив), Cut(пРив), Cut(Прив), Cut(прив)]                                                                                                                                                                           
DEBUG:rg::args: will try to use memory maps
```

Notably `Прив` is in the set (second from last), and searching that explicitly works:

```
$ rg 'Прив' test.txt                                                                                                                                                              
2:Привет
```

In fact, adding a `-i` flag works too:

```
$ rg 'Прив' test.txt -i
1:привет                                                                                                                                                                                             
2:Привет                                                                                                                                                                                             
3:ПрИвЕт
```

Regardless of what's going on, the following should *never* happen:

```
$ rg -i привет test.txt                                                                                                                                                           
$ rg привет test.txt 
1:привет
```

---

_Comment by @BurntSushi on 2016-11-25 14:56_

I'll be on vacation for the next few days, so unfortunately this bug will have to linger.

---

_Closed by @BurntSushi on 2016-11-28 23:32_

---

_Comment by @BurntSushi on 2016-11-28 23:32_

This was a subtly gross bug, but it should be fixed now. See the [commit message](https://github.com/BurntSushi/ripgrep/commit/0473df1ef5721143941fb7f883e22b17292b35bb) for details.

---
