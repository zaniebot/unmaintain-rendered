```yaml
number: 1573
title: Unexpected --count-matches result
type: issue
state: closed
author: knutwannheden
labels:
  - bug
assignees: []
created_at: 2020-05-08T06:54:09Z
updated_at: 2020-05-09T03:24:42Z
url: https://github.com/BurntSushi/ripgrep/issues/1573
synced_at: 2026-01-12T16:13:23Z
```

# Unexpected --count-matches result

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

I have a pattern (with look-around) which with `--count` reports `2` but with `--count-matches` reports `0`. This doesn't appear to make sense. I would expect ``--count-matches` to report a number at least as high as `--count`.

#### What are the steps to reproduce the behavior?

File `test.txt` with contents:
```
def A;
def B;
use A;
use B;
```

Ripgrep usage:
```bash
rg --pcre2 -U '(?s)def (\w+);(?=.*use \w+)' test.txt --count-matches
```

#### What is the actual behavior?

The output is `0` whereas the output for the same command with `--count` instead of `--count-matches` is `2`.

#### What is the expected behavior?

I would expect an output of `2` and generally the result of `--count-matches` to be equal to or greater than that of `--count`.


---

_Label `bug` added by @BurntSushi on 2020-05-08 12:07_

---

_Closed by @BurntSushi on 2020-05-09 03:24_

---
