```yaml
number: 2822
title: "ripgrep ignore files inside `.github` folder"
type: issue
state: closed
author: zufuliu
labels:
  - wontfix
assignees: []
created_at: 2024-05-27T10:27:33Z
updated_at: 2024-05-27T12:44:14Z
url: https://github.com/BurntSushi/ripgrep/issues/2822
synced_at: 2026-01-12T16:13:25Z
```

# ripgrep ignore files inside `.github` folder

---

_@zufuliu_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0 (rev e50df40a19)

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)

### How did you install ripgrep?

https://github.com/BurntSushi/ripgrep/releases/download/14.1.0/ripgrep-14.1.0-x86_64-pc-windows-msvc.zip

### What operating system are you using ripgrep on?

Windows 11 23H2

### Describe your bug.

ripgrep ignore files inside `.github` folder.

### What are the steps to reproduce the behavior?

```
D:\notepad4>mkdir test

D:\notepad4>cd test

D:\notepad4\test>git init .
Initialized empty Git repository in D:/notepad4/test/.git/

D:\notepad4\test>echo "#test" > test.yml

D:\notepad4\test>cp test.yml test2.yml

D:\notepad4\test>mkdir .github

D:\notepad4\test>cp test.yml .github/

D:\notepad4\test>rg test
test2.yml
1:"#test"

test.yml
1:"#test"

D:\notepad4\test>rg test .github
.github\test.yml
1:"#test"

D:\notepad4\test>git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .github/
        test.yml
        test2.yml

nothing added to commit but untracked files present (use "git add" to track)

D:\notepad4\test>rg --version
ripgrep 14.1.0 (rev e50df40a19)

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)

D:\notepad4\test>
```

### What is the actual behavior?

files inside `.github` folder are ignored by default.

### What is the expected behavior?

files inside `.github` folder should be searched by default.

---

_Renamed from "riggrep ignore files inside `.github` folder" to "ripgrep ignore files inside `.github` folder" by @zufuliu on 2024-05-27 10:29_

---

_Comment by @zufuliu on 2024-05-27 10:34_

I think the `.github` folder (e.g. https://github.com/BurntSushi/ripgrep/tree/master/.github) should not be ignored by default as it contains repository configuration files.

---

_Comment by @BurntSushi on 2024-05-27 12:22_

Literally the second sentence of ripgrep's docs say (emphasis mine):

> By default, ripgrep will respect gitignore rules and automatically **skip hidden files/directories** and binary files.

So `.github` gets skipped. If you want to search it, pass the `-./--hidden` flag, or add `!/.github/` to a `.ignore` file in the root of your repository.

---

_Closed by @BurntSushi on 2024-05-27 12:22_

---

_Label `wontfix` added by @BurntSushi on 2024-05-27 12:22_

---

_Comment by @zufuliu on 2024-05-27 12:26_

unlike `.git`, `.github` is not a hidden folder on Windows, and not added in `.gitignore`.
![image](https://github.com/BurntSushi/ripgrep/assets/2289926/f075c649-7d3f-493a-9e5a-43b039e9f6a4)


---

_Comment by @BurntSushi on 2024-05-27 12:32_

The work-around available to you is to add it to the `.ignore`. I'm not adding special rules to ripgrep for specific directories. That's why `.ignore` and `.rgignore` exists. So that you can define your own rules. And on Windows, it is my experience that `.` indicating a hidden file is a strong enough convention for ripgrep to respect it. Particularly given that ripgrep is probably most commonly used by programmers where the leading `.` convention is most likely to be seen. For example, you screenshot shows a number of files that aren't "hidden" but still have a leading `.`.

---

_Comment by @zufuliu on 2024-05-27 12:44_

OK, thanks for the help, I see your https://github.com/BurntSushi/ripgrep/blob/master/.ignore file.

---
