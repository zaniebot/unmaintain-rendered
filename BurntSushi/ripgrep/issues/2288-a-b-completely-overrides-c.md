```yaml
number: 2288
title: "-A/-B completely overrides -C"
type: issue
state: closed
author: jwilk
labels:
  - enhancement
assignees: []
created_at: 2022-08-22T06:29:42Z
updated_at: 2023-07-08T22:52:51Z
url: https://github.com/BurntSushi/ripgrep/issues/2288
synced_at: 2026-01-12T16:13:24Z
```

# -A/-B completely overrides -C

---

_@jwilk_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
```

#### How did you install ripgrep?

```
apt-get install ripgrep
```

#### What operating system are you using ripgrep on?

Debian unstable

#### Describe your bug.

The -A option completely overrides -C:

```console
$ seq 1 10 | rg 5 -C1 -A2
5
6
7
```

#### What is the expected behavior?

The -A option should affect only trailing context.
This is how it works in GNU grep:
```console
$ seq 1 10 | grep 5 -C1 -A2
4
5
6
7
```


---

_Comment by @BurntSushi on 2022-08-22 12:27_

Hmmm, interesting. I _think_ I agree with how grep does things here.

The docs are at least correct from my perspective though, so I think today's behavior matches the intended behavior. So I'll mark this as an enhancement. It will be a breaking change, but it's the kind of breaking change that I think makes sense and I'm okay making.

---

_Label `enhancement` added by @BurntSushi on 2022-08-22 12:27_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---
