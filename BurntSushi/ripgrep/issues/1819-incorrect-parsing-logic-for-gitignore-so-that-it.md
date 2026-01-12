```yaml
number: 1819
title: incorrect parsing logic for .gitignore so that it cannot support gitignore in whitelist mode
type: issue
state: closed
author: flw-cn
labels:
  - invalid
assignees: []
created_at: 2021-03-14T13:55:12Z
updated_at: 2021-03-14T15:04:55Z
url: https://github.com/BurntSushi/ripgrep/issues/1819
synced_at: 2026-01-12T16:13:24Z
```

# incorrect parsing logic for .gitignore so that it cannot support gitignore in whitelist mode

---

_@flw-cn_

#### What version of ripgrep are you using?

```
❯ rg --version
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`brew install rg`

#### What operating system are you using ripgrep on?

```
❯ uname -a
Darwin waker 19.6.0 Darwin Kernel Version 19.6.0: Tue Jan 12 22:13:05 PST 2021; root:xnu-6153.141.16~1/RELEASE_X86_64 x86_64
```

#### Describe your bug.

Some projects may generate messy files during the compilation process, or for other reasons, some developers prefer to whitelist rather than blacklist what files git needs to keep track of. In this case, the `.gitignore` file usually starts with `*` and then explicitly specifies the files you don't want to ignore in a whitelist.

At this point, rg will fail completely.
https://github.com/magiblot/tvision is one such project.

#### What are the steps to reproduce the behavior?

`git clone https://github.com/magiblot/tvision` and type `rg TApplication::TApplication` in its working directory, and you'll find that you get nothing. Instead, with `rg --no-ignore TApplication::TApplication` you find the following result (which is exactly what you would expect).

```
❯ rg --no-ignore TApplication::TApplication
source/tvision/tapplica.cpp
41:TApplication::TApplication() :
```

#### What do you think ripgrep should have done?

I would like rg to support whitelisting mode for `.gitignore`, which would mean processing the `.gitignore` from start to finish. Maybe this might affect performance, or maybe there is a workaround (like `git ls-files` or something like that), but in any case I want rg to work on this project, because I know some other developers who like to work this way.

Otherwise I'd have to use `rg --no-ignore` every time, which is a bit inconvenient.


---

_Comment by @BurntSushi on 2021-03-14 14:28_

ripgrep handles whitelisting just fine. If it didn't, then for example, `rg THelloApp` would return nothing in your repo too. But it finds results in whitelisted files.

The actual problem here is that your `.gitignore` has your `source` directory marked as ignored. There is no rule in your `.gitignore` file that whitelists the `source` directory. So ripgrep skips it.

Basically, in other words, your `.gitignore` is out of sync with the files actually checked into your repo. ripgrep _only_ looks at your `.gitignore` files. Not what's checked into your repo.

---

_Closed by @BurntSushi on 2021-03-14 14:28_

---

_Label `invalid` added by @BurntSushi on 2021-03-14 14:28_

---

_Comment by @flw-cn on 2021-03-14 15:04_

Thanks, I appreciate you helping me find the problem. I'll go communicate with that repository.

---
