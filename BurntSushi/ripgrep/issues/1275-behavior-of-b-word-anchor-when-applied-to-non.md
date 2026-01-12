```yaml
number: 1275
title: Behavior of \b word anchor when applied to non-word characters
type: issue
state: closed
author: learnbyexample
labels:
  - bug
  - rollup
assignees: []
created_at: 2019-05-08T12:53:22Z
updated_at: 2023-10-10T00:29:55Z
url: https://github.com/BurntSushi/ripgrep/issues/1275
synced_at: 2026-01-12T16:13:23Z
```

# Behavior of \b word anchor when applied to non-word characters

---

_@learnbyexample_

#### What version of ripgrep are you using?

```bash
$ rg --version
ripgrep 11.0.1 (rev 1f1cd9b467)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)
```

#### How did you install ripgrep?

Using https://github.com/BurntSushi/ripgrep/releases/download/11.0.1/ripgrep_11.0.1_amd64.deb

#### What operating system are you using ripgrep on?

Ubuntu 16.04.1

#### Describe your question, feature request, or bug.

Behavior of `\b` word anchor applied on non-word characters is different than what is observed on other regex engines. I found this accidentally, wasn't a usecase I needed. But filing the issue anyway, incase this may be a bug.

#### If this is a bug, what are the steps to reproduce the behavior?

See example below.

#### If this is a bug, what is the actual behavior?

```bash
$ echo 'I have 12, he has 2!' | rg -o '\b..\b'
I 
12
```

#### If this is a bug, what is the expected behavior?

I feel `ripgrep` should also behave as seen in other regex engines.

```bash
$ # GNU grep 3.3
$ # grep -oP '\b..\b' and rg -oP '\b..\b' also produce same result
$ echo 'I have 12, he has 2!' | grep -o '\b..\b'
I 
12
, 
he
 2

$ echo 'I have 12, he has 2!' | perl -lne 'print join "|", /\b..\b/g'
I |12|, |he| 2
$ echo 'I have 12, he has 2!' | ruby -ne 'puts $_.scan(/\b..\b/).join("|")'
I |12|, |he| 2

$ # python3.7
>>> import re
>>> re.findall(r'\b..\b', 'I have 12, he has 2!')
['I ', '12', ', ', 'he', ' 2']
```

The below image might help in understanding what is happening in all these regex engines. Vertical bar represents word boundary. Note that even though `!` is at end of line, it doesn't have boundary after as it is not a word character.

![ripgrep_word_boundary](https://user-images.githubusercontent.com/17766317/57372980-ce421500-71b4-11e9-8b12-e44ad4fd5532.png)

**Note**

* `grep -o '\<..\>'` (also in `vim`) will be different compared to `\b` as `\b` cannot differentiate between start and end word boundary
* `grep -ow '..'` is same as `rg -ow '..'` (perhaps because both do `(?<!\w)pattern(?!\w)` for `-w` option)



---

_Comment by @BurntSushi on 2019-05-08 13:11_

Thanks for the report! This smells like a bug in the regex engine. I've filed an issue for it here: https://github.com/rust-lang/regex/issues/579

---

_Label `bug` added by @BurntSushi on 2019-05-08 13:11_

---

_Label `rollup` added by @BurntSushi on 2023-10-09 23:45_

---

_Closed by @BurntSushi on 2023-10-10 00:29_

---
