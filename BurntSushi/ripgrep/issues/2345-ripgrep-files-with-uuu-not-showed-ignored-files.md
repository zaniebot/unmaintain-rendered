```yaml
number: 2345
title: ripgrep --files with -uuu not showed ignored files
type: issue
state: closed
author: JMarkin
labels: []
assignees: []
created_at: 2022-11-06T19:18:15Z
updated_at: 2022-11-06T22:07:31Z
url: https://github.com/BurntSushi/ripgrep/issues/2345
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep --files with -uuu not showed ignored files

---

_@JMarkin_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

cargo install ripgrep

#### What operating system are you using ripgrep on?

archlinux, kernel 6.0.6

#### Describe your bug.
Started ripgrep with --files and -uuu I didn't visible ignored files.

#### What are the steps to reproduce the behavior?
```bash
[kron@kron-pc front]$ ls
build  dist  node_modules  package.json  package-lock.json  src 
[kron@kron-pc front]$ cat .gitignore | grep dist
dist/
[kron@kron-pc front]$ rg --files -uuu | grep dist
[kron@kron-pc front]$ fd -I | grep dist/
dist/

```
#### What is the actual behavior?

https://gist.github.com/JMarkin/58a135e49b6bfded041e67172d3a5dac

#### What is the expected behavior?
I see 'dist' in search

What do you think ripgrep should have done?


---

_Comment by @BurntSushi on 2022-11-06 19:29_

Your debug output doesn't say it's ignoring the `dist` directory, which makes me suspicious.

Can you reproduce this on a corpus we both can access please?

---

_Comment by @JMarkin on 2022-11-06 21:08_

I tried create archive for reproduce. Found, dist folder is empty :D Understand why dist not working.

But node_modules not empty and `rg --files -uuu | grep node_modules` also nothing...
In previous gist have logs about node_modules. Can it help you?
I created small tar archive https://disk.yandex.ru/d/tBCG8eUbk6ER5Q . I can to reproduce on other machine


---

_Comment by @BurntSushi on 2022-11-06 21:22_

My best guess is that everything is symlinked and ripgrep doesn't traverse symlinks by default. You'll want `-L` for that.

---

_Closed by @JMarkin on 2022-11-06 21:30_

---

_Locked by @ghost on 2022-11-06 22:07_

---
