```yaml
number: 1084
title: "Correctness issue: pattern doesn't match when it should"
type: issue
state: closed
author: khyperia
labels:
  - duplicate
assignees: []
created_at: 2018-10-16T16:10:59Z
updated_at: 2018-12-06T18:45:21Z
url: https://github.com/BurntSushi/ripgrep/issues/1084
synced_at: 2026-01-12T16:13:22Z
```

# Correctness issue: pattern doesn't match when it should

---

_@khyperia_

#### What version of ripgrep are you using?

```
% rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### What operating system are you using ripgrep on?

Arch Linux, using the Arch-official `ripgrep` package.

#### Bug description/reproduction steps

This prints out "abcd".

`echo 'abcd' | rg 'a(.*(d|ce))'`

Adding a "c" makes it not match (i.e. it doesn't print out anything).

`echo 'abcd' | rg 'a(.*(cd|ce))'`

#### If this is a bug, what is the expected behavior?

I believe that both regexes should match the input, and print out a match in both cases. Testing using [regex101](https://regex101.com/) seems to imply this is true.

#### Real-world discovery background (non-minimal case that made me discover the minimal repro above):

I was trying to run `git diff --name-only master... | rg 'js/src/(.*(cpp|\.h))' --replace '$1'`.

`git diff --name-only master...` printed lines such as:

```
js/src/frontend/BinSource.yaml
js/src/frontend/BytecodeEmitter.cpp
js/src/frontend/ParseNode.h
```

and I wanted to extract:

```
frontend/BytecodeEmitter.cpp
frontend/ParseNode.h
```

(i.e. trim `js/src/` off the front, and only match `cpp` and `h` files)

However, adding a `\.` in front of the `cpp` made *nothing* match.

`git diff --name-only master... | rg 'js/src/(.*(\.cpp|\.h))' --replace '$1'` -- prints nothing???

---

_Comment by @BurntSushi on 2018-10-16 16:27_

This is a duplicate of #1064 and #1081. It's fixed on master.

From my digging, I thought that this bug wasn't a regression, but had always been there in previous versions of ripgrep. However, we've had this bug filed three times since the release of ripgrep 0.10, but never before. So something seems fishy.

---

_Closed by @BurntSushi on 2018-10-16 16:27_

---

_Label `duplicate` added by @BurntSushi on 2018-10-16 16:27_

---

_Comment by @AntCrow on 2018-12-06 18:45_

FWIW, I don't think it's a regression. I found the same thing on 0.8.1, confirmed it on 0.10.0, and was going to report it until I found all the duplicates. 

---
