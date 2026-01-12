```yaml
number: 1349
title: "ignore: include files below previously excluded path / (nested) whitelist"
type: issue
state: closed
author: blueyed
labels:
  - bug
  - wontfix
assignees: []
created_at: 2019-08-15T16:30:02Z
updated_at: 2019-08-15T17:53:57Z
url: https://github.com/BurntSushi/ripgrep/issues/1349
synced_at: 2026-01-12T16:13:23Z
```

# ignore: include files below previously excluded path / (nested) whitelist

---

_@blueyed_

I want to include files from a previously ignored directory ("build", via
"/build/" in `.gitignore`).

It would be great if the following pattern worked in an `.ignore` file then:

```
!/build/src/nvim/auto/**
```

But what is necessary currently is:

```
!/build
/build/**
!/build/src
!/build/src/nvim
!/build/src/nvim/auto
!/build/src/nvim/auto/**
```

Or:

```
!/build
/build/*
!/build/src
/build/src/*
!/build/src/nvim
/build/src/nvim/*
!/build/src/nvim/auto
```

I.e. first do not include the directory as-is, and then ignore subentries in
all of them, but whitelist the subpaths down to the subdirectory.

It would be nice if ripgrep could transform the entry
`!/build/src/nvim/auto/**`automatically into these (internal) patterns.

While the use case here is "overriding something from a .gitignore" file, it is a useful pattern by itself: ignore everything except for some files from (a nested subdirectory of) a directory.

(As a single regular expressions `'build/(?!src/nvim/auto/)'` could be used here.)

The main problem probably is that subdirectories are not descended into in the first place, but here any whitelists could/should be considered then maybe?

#### What version of ripgrep are you using?

    ripgrep 11.0.2
    -SIMD -AVX (compiled)
    +SIMD +AVX (runtime)


---

_Comment by @BurntSushi on 2019-08-15 16:52_

> The main problem probably is that subdirectories are not descended into in the first place

Yes, that is indeed the problem. I unfortunately do not see this getting fixed, although I do agree it is unfortunate behavior.

---

_Closed by @BurntSushi on 2019-08-15 16:52_

---

_Label `bug` added by @BurntSushi on 2019-08-15 16:52_

---

_Label `wontfix` added by @BurntSushi on 2019-08-15 16:52_

---

_Comment by @blueyed on 2019-08-15 17:40_

Wouldn't it be an option then to check whitelists when traversing directories?

---

_Comment by @BurntSushi on 2019-08-15 17:53_

I'm not adding code that automatically determines whitelists like this, sorry.

---
