```yaml
number: 2414
title: "Literal string flag `-F` doesn't escape ripgrep flags"
type: issue
state: closed
author: kwaegel
labels:
  - wontfix
assignees: []
created_at: 2023-02-09T20:48:11Z
updated_at: 2023-02-09T21:43:06Z
url: https://github.com/BurntSushi/ripgrep/issues/2414
synced_at: 2026-01-12T16:13:24Z
```

# Literal string flag `-F` doesn't escape ripgrep flags

---

_@kwaegel_

#### What version of ripgrep are you using?
`13.0.0`

#### How did you install ripgrep?
`cargo install ripgrep`

#### What operating system are you using ripgrep on?
Ubuntu 22.04 under WSL.

#### Describe your bug.
The `--fixed-strings`/`-F` flag does not properly search for strings that are also ripgrep flags.

#### What are the steps to reproduce the behavior?
```
rg --fixed-strings "--version"
```
Similar behavior when searching for any other valid ripgrep flag.

#### What is the actual behavior?
```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### What is the expected behavior?
ripgrep does a search for the literal string `--version`.

#### Current workaround
Use a regex with escape characters.
```
rg "\--version"
```


---

_Comment by @BurntSushi on 2023-02-09 21:42_

The `-F` flag ia for escaping regex meta characters. What you want is escaping special characters with respect to the argv parser. They are two different things.

As documented in the man page and the `--help` output, you need to do `-e --version` or `-- --version`. You can also escape them as you've done or even `[-]-version`.

---

_Closed by @BurntSushi on 2023-02-09 21:42_

---

_Label `wontfix` added by @BurntSushi on 2023-02-09 21:43_

---
