```yaml
number: 627
title: equivalence option
type: issue
state: closed
author: albfan
labels:
  - question
assignees: []
created_at: 2017-10-07T09:54:27Z
updated_at: 2024-06-14T17:14:25Z
url: https://github.com/BurntSushi/ripgrep/issues/627
synced_at: 2026-01-12T16:13:22Z
```

# equivalence option

---

_@albfan_

just if #626 is possible

Using kcharselect o gucharmap and looking for "letter with" you will find every unicode character that can have accents, like aáà,eéè etc. In spanish it works mostly for vocals, but seems there're other languages with consonants modyfied with accents. Something like --equivalents to search for:

    $ echo película sobre un Camión español | grep -i "pel[[=i=]]cula sobre un cami[[=o=]]n espa[[=n=]]ol"

will rock


---

_Comment by @BurntSushi on 2017-10-07 11:58_

I don't understand what the difference is between this issue and #626. Could you please elaborate?

---

_Label `question` added by @BurntSushi on 2017-10-07 11:59_

---

_Comment by @albfan on 2017-10-07 20:15_

#626 is to support the regex

    [=a=] equals to [aáàä]

this is to allow to write non accent characters in regex and match with or without it

    rg --equivalents camion matches camión and camion

For long phrases is just syntastic sugar

    Tomás pidió públicamente perdón, disculpándose después muchísimo más íntimamente



---

_Comment by @BurntSushi on 2017-10-08 00:08_

This doesn't seem unreasonable to me, but I would rather wait until the equivalence feature exists in the regex engine before tracking additional features on top of that.

---

_Closed by @BurntSushi on 2017-10-08 00:08_

---

_Comment by @albfan on 2017-10-08 14:45_

Sure

---

_Comment by @albfan on 2017-11-05 23:49_

this would be another task of preprocessing, but this time for regex expression instead of files searched.

---

_Comment by @HynDuf on 2024-06-14 17:10_

I would love this too.

---
