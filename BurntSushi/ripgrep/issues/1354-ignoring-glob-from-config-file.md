```yaml
number: 1354
title: Ignoring glob from config file
type: issue
state: closed
author: pcdevil
labels:
  - question
assignees: []
created_at: 2019-08-21T13:56:23Z
updated_at: 2019-08-21T18:35:37Z
url: https://github.com/BurntSushi/ripgrep/issues/1354
synced_at: 2026-01-12T16:13:23Z
```

# Ignoring glob from config file

---

_@pcdevil_

#### What version of ripgrep are you using?

```bash
$ rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)
```

#### How did you install ripgrep?

```bash
brew install ripgrep
```

#### What operating system are you using ripgrep on?

MacOS 18.7.0

#### Describe your question, feature request, or bug.

**Context**:

I have a `.ripgreprc` file where I defined some globs, such as
```
--glob=!**/test
```

But there are cases I want to search in all folders, including the test folder.

**Question**:

I am familiar with the `-u` and the `-uuu` parameters. Is there a way to _ignore_ globs in some cases? Like a `-uuuu` parameter?

If not, do you have any suggestion how to resolve this?

#### If this is a bug, what are the steps to reproduce the behavior?

N/A

#### If this is a bug, what is the actual behavior?

N/A

#### If this is a bug, what is the expected behavior?

N/A

---
Thanks,   
Attila

---

_Comment by @BurntSushi on 2019-08-21 14:03_

I suppose you could just do `--glob '**/test'`. That might override the `!`.

Short of that, no. I'd recommend using a `.ignore` or a `.rgignore` to ignore `test` directories/files.

---

_Comment by @pcdevil on 2019-08-21 14:15_

Thanks for the quick reply!

Actually `.rgignore` is a much cleaner solution for my problem :)

Can I create a global `.rgignore` file, like the `.ripgreprc`, for example a `RIPGREP_IGNORE_PATH`? I didn't find any reference in the guide.

---

_Comment by @BurntSushi on 2019-08-21 14:56_

Yes. The `--ignore-file` flag can be provided. You can use that in your `.ripgreprc` and have it point to an ignore file that is used on every invocation.

---

_Closed by @BurntSushi on 2019-08-21 14:56_

---

_Label `question` added by @BurntSushi on 2019-08-21 14:56_

---

_Comment by @pcdevil on 2019-08-21 17:11_

Thanks again!

Although it seems to work, I am getting an
```bash
~/.rgignore: No such file or directory (os error 2) 
```

error. Should I file a bug report on it, or I mis-configured something?

---

_Comment by @BurntSushi on 2019-08-21 17:15_

There is no bug. That file just doesn't exist.

Please keep in mind that the special alias `~` isn't always resolved in the way you probably expect it to. Your shell typically expands `~`, but your shell isn't interpreting your `.ripgreprc`.

---

_Comment by @pcdevil on 2019-08-21 18:35_

Okay I know why I see it's working:   
The folder I was testing is inside the home directory and ripgrep also scans the parents for `.rgignore`. That was confusing a bit.

Lesson learnt. Now I have to figure out how to deal with the `~` problem without aliasing the rg command.. :)

Again, thanks for the help!

---
