```yaml
number: 1879
title: Second line is matching with multiline option when trying to match start of entire input
type: issue
state: closed
author: learnbyexample
labels:
  - duplicate
assignees: []
created_at: 2021-05-29T11:29:39Z
updated_at: 2021-05-29T11:43:11Z
url: https://github.com/BurntSushi/ripgrep/issues/1879
synced_at: 2026-01-12T16:13:24Z
```

# Second line is matching with multiline option when trying to match start of entire input

---

_@learnbyexample_

#### What version of ripgrep are you using?

`ripgrep 12.1.1 (rev 7cb211378a)`

#### How did you install ripgrep?

Using `ripgrep_12.1.1_amd64.deb`

#### What operating system are you using ripgrep on?

Ubuntu 20.04 LTS

#### Describe your bug.

Was trying to create an example to show difference between `\A` and `^` anchors when multiline option is enabled. For certain input, `\A` is matching second line content.

#### What is the actual behavior?

Example with `\A` string anchor. The last command gives no output as expected.

```bash
$ printf '1\nbaz\nabc\n' | rg -U '\Ab'
baz
$ printf '12\nbaz\nabc\n' | rg -U '\Ab'
baz
$ printf '123\nbaz\nabc\n' | rg -U '\Ab'
$ 
```

Same thing seen with `^` anchor and `m` flag disabled:

```bash
$ printf '1\nbaz\nabc\n' | rg -U '(?-m)^b'
baz
$ printf '12\nbaz\nabc\n' | rg -U '(?-m)^b'
baz
$ printf '123\nbaz\nabc\n' | rg -U '(?-m)^b'
$ 
```

#### What is the expected behavior?

Second line shouldn't match for above cases.


---

_Closed by @BurntSushi on 2021-05-29 11:38_

---

_Label `duplicate` added by @BurntSushi on 2021-05-29 11:38_

---

_Comment by @BurntSushi on 2021-05-29 11:38_

I beat you to it: #1878 :-)

Fix is on master.

---

_Comment by @learnbyexample on 2021-05-29 11:43_

Wow, you saw the comment, filed an issue and fixed it while I was still trying to generate more test cases (I was about to edit my issue to add those when I saw it was closed already!).

---
