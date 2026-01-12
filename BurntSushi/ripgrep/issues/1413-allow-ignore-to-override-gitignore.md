```yaml
number: 1413
title: Allow .ignore to override .gitignore
type: issue
state: closed
author: tbodt
labels:
  - invalid
assignees: []
created_at: 2019-10-29T18:23:46Z
updated_at: 2019-10-29T18:32:58Z
url: https://github.com/BurntSushi/ripgrep/issues/1413
synced_at: 2026-01-12T16:13:23Z
```

# Allow .ignore to override .gitignore

---

_@tbodt_

#### What version of ripgrep are you using?

ripgrep 11.0.1 (rev 1f1cd9b467)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### What operating system are you using ripgrep on?

Debian

#### Describe your question, feature request, or bug.

I have some files in .gitignore that I want to search anyway. (In this case they're git repositories inside a main git repository that are managed by a script rather than as submodules.) --no-ignore-vcs excludes the entire gitignore, which I don't want because that includes things like build directories. I just want to whitelist some directories, which I was thinking I could do with a .ignore pattern like `!/third_party`. 

---

_Comment by @BurntSushi on 2019-10-29 18:28_

`.rgignore` overrides `.ignore` and `.ignore` overrides `.gitignore`. So this is already possible.

---

_Closed by @BurntSushi on 2019-10-29 18:28_

---

_Label `invalid` added by @BurntSushi on 2019-10-29 18:28_

---

_Comment by @tbodt on 2019-10-29 18:30_

I tried it and it didn't work. So there might be a bug or I might be on an old version. I'll investigate.

---

_Comment by @tbodt on 2019-10-29 18:32_

Aha, my .gitignore had `/third_party/*` instead of `/third_party`. Works now.

---
