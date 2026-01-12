```yaml
number: 2489
title: "ignorefiles aren't respected when searched with directory"
type: issue
state: closed
author: okuramasafumi
labels:
  - duplicate
assignees: []
created_at: 2023-04-13T10:19:08Z
updated_at: 2023-04-13T11:01:08Z
url: https://github.com/BurntSushi/ripgrep/issues/2489
synced_at: 2026-01-12T16:13:24Z
```

# ignorefiles aren't respected when searched with directory

---

_@okuramasafumi_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`brew install rg -s`

#### What operating system are you using ripgrep on?

`macOS 13.2.1`

#### Describe your bug.

Say we have a directory structure like this:

```
/a/b
/a/c
/z
```

And `.gitignore` looks like this:

```
/a/b
```

And when we run `rg 'something' a`, the content from `a/b` appears. However, when we run `rg 'something'`, the content from a/b` doesn't appear.

#### What are the steps to reproduce the behavior?

```sh
git init
mkdir -p a/b
mkdir a/c
mkdir z
echo "content" >> a/b/sample.txt
echo "content" >> a/c/sample.txt
echo "content" >> z/sample.txt
echo "/a/b" >> .gitignore
rg "content" a
```

#### What is the actual behavior?

```
a/b/sample.txt
1:content

a/c/sample.txt
1:content
```

#### What is the expected behavior?

```
a/c/sample.txt
1:content
```

---

_Label `duplicate` added by @BurntSushi on 2023-04-13 11:00_

---

_Comment by @BurntSushi on 2023-04-13 11:01_

This is a duplicate of a long standing bug. I'm on mobile or else I would dig it up.

---

_Closed by @BurntSushi on 2023-04-13 11:01_

---
