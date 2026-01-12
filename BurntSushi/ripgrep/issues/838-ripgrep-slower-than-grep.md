```yaml
number: 838
title: ripgrep slower than grep
type: issue
state: closed
author: bbbsg
labels:
  - bug
  - icebox
assignees: []
created_at: 2018-02-27T17:17:56Z
updated_at: 2019-04-07T23:11:04Z
url: https://github.com/BurntSushi/ripgrep/issues/838
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep slower than grep

---

_@bbbsg_

#### What version of ripgrep are you using?

ripgrep 0.8.1 (rev c8e9f25b85)
+SIMD -AVX

#### What operating system are you using ripgrep on?

Debian Stretch 64 bit

#### Describe your question, feature request, or bug.

ripgrep is at least 4 times slower than grep on a certain use case (never waited long enough for it to return). See data with test files attached.
[ripgrep_data_test.tar.gz](https://github.com/BurntSushi/ripgrep/files/1764089/ripgrep_data_test.tar.gz)


attached file is around 15 mb uncompressed, may be possible to reduce the sample further but the original files i was dealing with were 100X.

See README.md and test.sh , i tried with and without --dfa-size-limit

Another minor issue I noticed is that a hint to use --fixed-strings us provided to the user even when using -F already, perhaps this only happens when regex-size-limit is not passed in.

Ideally ripgrep would be faster than grep :-)


#### If this is a bug, what are the steps to reproduce the behavior?

Assumes rg is in the directory or already in the path, then run test.sh



---

_Label `bug` added by @BurntSushi on 2018-02-27 17:58_

---

_Comment by @BurntSushi on 2018-02-27 17:59_

Thanks for the bug report! I was able to reproduce this. It looks like most of the time spent here is just compiling the patterns (since there are >100K). As you pointed out on IRC, using Aho-Corasick here should help quite a bit.

---

_Label `libripgrep` added by @BurntSushi on 2018-03-10 13:04_

---

_Added to milestone `libripgrep` by @BurntSushi on 2018-03-10 13:04_

---

_Label `libripgrep` removed by @BurntSushi on 2018-08-19 14:05_

---

_Removed from milestone `libripgrep` by @BurntSushi on 2018-08-19 14:05_

---

_Label `icebox` added by @BurntSushi on 2019-01-27 18:12_

---

_Closed by @BurntSushi on 2019-04-07 23:11_

---
