```yaml
number: 30
title: "trailing recursive globs shouldn't ignore the directory itself, only its contents"
type: issue
state: closed
author: foophoof
labels:
  - bug
assignees: []
created_at: 2016-09-23T18:54:29Z
updated_at: 2016-09-24T23:44:10Z
url: https://github.com/BurntSushi/ripgrep/issues/30
synced_at: 2026-01-12T18:23:11Z
```

# trailing recursive globs shouldn't ignore the directory itself, only its contents

---

_@foophoof_

I have a .gitignore file that contains something like this:

```
vendor/**
!vendor/manifest
```

I'd expect `vendor/manifest` to be searched, but nothing else in the `vendor` directory, but instead it seems like `vendor/manifest` is ignored too. If I remove the `vendor/**` line, the search does what I expect it to.


---

_Comment by @BurntSushi on 2016-09-24 02:00_

This is an interesting case, because `rg` will avoid descending into the `vendor`, which appears wrong, since `vendor` itself shouldn't be ignored. From `man gitignore`:

```
       ·   A trailing "/**" matches everything inside. For example, "abc/**" matches all files inside directory "abc",
           relative to the location of the .gitignore file, with infinite depth.
```

(Running `rg` with the `--debug` flag told me that `vendor` itself was ignored. `rg` never even sees `vendor/manifest`.)


---

_Renamed from "Negated gitignore patterns aren't supported" to "trailing recursive globs shouldn't ignore the directory itself, only its contents" by @BurntSushi on 2016-09-24 02:01_

---

_Label `bug` added by @BurntSushi on 2016-09-24 02:01_

---

_Closed by @BurntSushi on 2016-09-24 23:44_

---
