```yaml
number: 1572
title: "ripgrep doesn't handle overlapping multiline searches"
type: issue
state: closed
author: knutwannheden
labels:
  - invalid
assignees: []
created_at: 2020-05-08T04:56:39Z
updated_at: 2020-05-08T10:37:05Z
url: https://github.com/BurntSushi/ripgrep/issues/1572
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep doesn't handle overlapping multiline searches

---

_@knutwannheden_

#### What version of ripgrep are you using?

```
ripgrep 12.0.1 (rev 1d5b1011e5)
-SIMD -AVX (compiled)
```

#### How did you install ripgrep?

Choco

#### What operating system are you using ripgrep on?

Windows 10

#### Describe your bug.

I am searching files for multiline patterns which can be overlapping and ripgrep only reports the first match in these cases.

#### What are the steps to reproduce the behavior?

Text file `test.txt`:
```
def A;
def B;
use A;
use B;
```

Ripgrep usage:
```bash
rg -U '(?s)def \w+;.*use \w+;' test.txt --count-matches
```

#### What is the actual behavior?

The output from the above is `1`.

#### What is the expected behavior?

I would have expected the output `2`.


---

_Comment by @knutwannheden on 2020-05-08 05:30_

Of course the problem is with `--count-matches` rather than `--count`. I have updated the issue description.

---

_Comment by @knutwannheden on 2020-05-08 06:45_

After some more thinking I have concluded that what I am asking doesn't really make sense. The problem should be solved using positive lookahead (or lookbehind) as in e.g. `rg -U '(?s)def \w+;(?=.*use \w+;)' test.txt`.

---

_Closed by @knutwannheden on 2020-05-08 06:45_

---

_Label `invalid` added by @BurntSushi on 2020-05-08 10:37_

---
