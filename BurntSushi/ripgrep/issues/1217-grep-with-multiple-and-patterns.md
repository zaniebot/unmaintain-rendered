```yaml
number: 1217
title: grep with multiple AND patterns
type: issue
state: closed
author: moroal
labels:
  - duplicate
assignees: []
created_at: 2019-03-11T01:12:20Z
updated_at: 2019-03-13T11:47:49Z
url: https://github.com/BurntSushi/ripgrep/issues/1217
synced_at: 2026-01-12T16:13:23Z
```

# grep with multiple AND patterns

---

_@moroal_

#### What version of ripgrep are you using?
0.10.0

#### How did you install ripgrep?
unzip binary releases

#### What operating system are you using ripgrep on?
win7

#### Describe your question, feature request, or bug.
feature request：grep with multiple AND patterns
such as :  rg word1&word2&word3
not : rg word1 | rg word2 | rg word3
because the second one prints the file path as a prefix for each matched line instead of printing the file path above clusters of matches from each file


---

_Comment by @BurntSushi on 2019-03-11 01:20_

Dupe of https://github.com/BurntSushi/ripgrep/issues/875

My feeling is that this is unlikely to happen. You'll just have to deal with the traditional output format.

---

_Closed by @BurntSushi on 2019-03-11 01:20_

---

_Label `duplicate` added by @BurntSushi on 2019-03-11 01:20_

---

_Comment by @moroal on 2019-03-12 01:28_

Thanks for your reply.

Is there any way to print the file path above clusters of matches from each file and to color each keyword?

> rg word1 | rg word2 | rg word3

expect：
file1
xxxx **word1 word2 word3** xxxx
xxxxxxx **word1** xxxxxxx  **word2**  xxxx **word3** 
file2
xxxx **word1 word2 word3** xxxx
xxxxxxx **word1** xxxxxxx  **word2**  xxxx **word3** 

not

file1:xxxx word1 word2 **word3** xxxx
file1:xxxxxxx word1 xxxxxxx  word2  xxxx **word3** 
file2:xxxx word1 word2 **word3** xxxx
file2:xxxxxxx word1 xxxxxxx  word2  xxxx **word3** 

May I have some suggestions on how to use it? Thank you.

---

_Comment by @BurntSushi on 2019-03-12 02:12_

No, not unless you're okay with an OR instead of an AND. Otherwise, this is the key downside of using a format that isn't line oriented. Namely, it is not composable.

---

_Comment by @moroal on 2019-03-13 11:47_

Okay, but I'm still a ripgrep fan.

---
