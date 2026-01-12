```yaml
number: 1520
title: warning about commondir on ripgrep over git repo with submodules
type: issue
state: closed
author: Roguelazer
labels:
  - bug
assignees: []
created_at: 2020-03-16T22:04:23Z
updated_at: 2020-03-29T23:20:29Z
url: https://github.com/BurntSushi/ripgrep/issues/1520
synced_at: 2026-01-12T16:13:23Z
```

# warning about commondir on ripgrep over git repo with submodules

---

_@Roguelazer_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 12.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

Also,

```
$ git --version
git version 2.25.1
```

#### How did you install ripgrep?

Homebrew

#### What operating system are you using ripgrep on?

macOS. Also seems to occur on Linux.

#### Describe your question, feature request, or bug.

When running ripgrep on a git clone with submodules, it prints the following error:

```
 $ rg -i foobar
../../../../.git/modules/path/to/submodule/commondir: No such file or directory (os error 2)
```

I suspect this is related to #1446 

#### If this is a bug, what are the steps to reproduce the behavior?

```
git init foo
git init bar
cd foo
touch file
git add file
git commit -m 'initial commit'
cd ../bar
touch file
git add file
git commit -m 'initial commit'
git submodule add ../foo path
rg string
```

#### If this is a bug, what is the actual behavior?


The ripgrep invocation will print

```
../.git/modules/path/commondir: No such file or directory (os error 2)
```

#### If this is a bug, what is the expected behavior?

No warnings. Never warnings.

---

_Closed by @BurntSushi on 2020-03-16 22:50_

---

_Comment by @BurntSushi on 2020-03-16 22:50_

Oof, sorry. You're right. This is a regression.

I will put out a new release soonish. I'll give it a few days in case any other regressions pop up.

---

_Label `bug` added by @BurntSushi on 2020-03-16 22:51_

---

_Comment by @joshuarubin on 2020-03-25 17:32_

Any chance you could cut a release now? This issue is driving me batty!

---

_Comment by @BurntSushi on 2020-03-25 17:33_

I'd recommend just building from source if it's that bad. Otherwise: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#release

---

_Comment by @BurntSushi on 2020-03-29 23:20_

I've released ripgrep 12.0.1 with a fix for this.

---
