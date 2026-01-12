```yaml
number: 1077
title: Support Double Dashes Before Final Argument
type: issue
state: closed
author: Miserlou
labels:
  - invalid
assignees: []
created_at: 2018-10-04T17:08:17Z
updated_at: 2018-10-04T17:19:33Z
url: https://github.com/BurntSushi/ripgrep/issues/1077
synced_at: 2026-01-12T16:13:22Z
```

# Support Double Dashes Before Final Argument

---

_@Miserlou_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?
brew

#### What operating system are you using ripgrep on?

OSX

#### Describe your question, feature request, or bug.

`rg` doesn't appear to support ` -- ` to signify the final argument:

ex: 
```
$ rg -- Client struct
struct: No such file or directory (os error 2)
```

I'd _expect_ anything after ` -- ` to be treated as the PATTERN parameter, ex:

```
$ rg -- Client struct
nomad/vault.go
165:type vaultClient struct {

api/api.go
336:type Client struct {
```

I got [pestered into adding this](https://github.com/Miserlou/Loop/issues/16) into my own Rust tool, `loop`, so I know it's possible! :-P

---

_Comment by @BurntSushi on 2018-10-04 17:14_

From the man page, ripgrep's usage:

```
rg [OPTIONS] PATTERN [PATH...]
```

Thus, if you give `rg -- Client struct`, then the expectation is that it would work as if you used `rg Client struct`. Since there are two positional parameters, the first is treated as a pattern and the second is treated as a path. Therefore, the error message you get is the expected behavior.

You probably want `rg -- 'Client struct'`.

Note that the purpose of `--` is not to "signify the final argument," but rather, to denote that anything following `--` should be interpreted as a _positional_ argument and _never_ a flag. This for example permits using `rg -- -e` to search for `-e` where `rg -e` would emit an error because it interprets `-e` as a flag. (`rg -e -e` does work though, so the `--` for this case isn't strictly necessary.)

---

_Closed by @BurntSushi on 2018-10-04 17:14_

---

_Label `invalid` added by @BurntSushi on 2018-10-04 17:14_

---

_Comment by @Miserlou on 2018-10-04 17:19_

Gotcha, thank you.

---
