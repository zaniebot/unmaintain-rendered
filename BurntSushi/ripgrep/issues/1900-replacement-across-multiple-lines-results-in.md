```yaml
number: 1900
title: Replacement across multiple lines results in excess output
type: issue
state: closed
author: learnbyexample
labels: []
assignees: []
created_at: 2021-06-17T12:00:50Z
updated_at: 2023-12-11T06:29:14Z
url: https://github.com/BurntSushi/ripgrep/issues/1900
synced_at: 2026-01-12T16:13:24Z
```

# Replacement across multiple lines results in excess output

---

_@learnbyexample_

#### What version of ripgrep are you using?

```bash
$ rg --version
ripgrep 13.0.0 (rev 7ec2fd51ba)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)
```

#### How did you install ripgrep?

Using the `deb` file.

#### What operating system are you using ripgrep on?

Ubuntu 20.04.2 LTS

#### Describe your bug.

Replacement across multiple lines isn't working as expected if there is input content after the matched group of lines.

#### What are the steps to reproduce the behavior?

```bash
$ s='foo\nhi there\nhave a nice day\nbye\n123\n456\n'

$ printf '%b' "$s" | rg -U '(?s)the.*ice' -r 'ZZZ'
hi ZZZ day
bye
123
456

$ printf '%b' "$s" | rg -U --passthru '(?s)the.*ice' -r 'ZZZ'
foo
hi ZZZ day
bye
123
456
bye
123
456

# multiple replacement example
$ printf '%b' "$s" | rg -U '(?s)oo.*the|ye.*2' -r 'XYZ'
fXYZre
have a nice day
bXYZ3
456
bXYZ3
456
$ printf '%b' "$s" | rg -U '(?s)oo.*the|ye.*2' -r 'XYZ' --passthru
fXYZre
have a nice day
bXYZ3
456
have a nice day
bXYZ3
456
456
```

#### What is the expected behavior?

* Without `--passthru` I do not expect remaining input lines to be part of the output or have some of the lines repeated
* With `--passthru` I do not expect output to have some of the lines repeated

---

_Comment by @learnbyexample on 2023-12-11 06:29_

Just checked with `14.0.3` version and these are working as expected.

---

_Closed by @learnbyexample on 2023-12-11 06:29_

---
