```yaml
number: 485
title: "Unrecognized file type: gradle"
type: issue
state: closed
author: d53dave
labels: []
assignees: []
created_at: 2017-05-24T08:08:56Z
updated_at: 2017-05-24T08:30:45Z
url: https://github.com/BurntSushi/ripgrep/issues/485
synced_at: 2026-01-12T16:13:22Z
```

# Unrecognized file type: gradle

---

_@d53dave_

Hi, 

I wanted to search for a specific dependency inside multiple gradle subprojects. Naturally, I tried 

```rg -tgradle derp``` which printed  ```unrecognized file type: gradle```

which is weird because `rg --type-list` prints

```
[...]
fsharp: *.fs, *.fsi, *.fsx
go: *.go
groovy: *.gradle, *.groovy
h: *.h, *.hpp
haskell: *.hs, *.lhs
hbs: *.hbs
[...]
```

I am using ripgrep 0.5.2 on MacOS 10.12.4. Am I missing something here?

---

_Comment by @d53dave on 2017-05-24 08:30_

My bad. It should be ```rg -tgroovy```.

---

_Closed by @d53dave on 2017-05-24 08:30_

---
