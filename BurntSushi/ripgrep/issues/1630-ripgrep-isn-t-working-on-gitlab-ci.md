```yaml
number: 1630
title: "ripgrep isn't working on GitLab CI"
type: issue
state: closed
author: ghost
labels:
  - invalid
assignees: []
created_at: 2020-06-27T11:42:01Z
updated_at: 2020-06-27T12:49:04Z
url: https://github.com/BurntSushi/ripgrep/issues/1630
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep isn't working on GitLab CI

---

_@ghost_

#### What version of ripgrep are you using?

```
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

```
apt-get install -y ripgrep strace
```

#### What operating system are you using ripgrep on?

```
Running with gitlab-runner 13.1.0 (6214287e)
  on docker-auto-scale ed2dce3a
Preparing the "docker+machine" executor
Using Docker executor with image ubuntu:20.04 ...
```

#### Describe your bug.

ripgrep doesn't show anything on GitLab CI and just quit.

#### What are the steps to reproduce the behavior?

Use this `gitlab-ci.yml` and run the pipeline.
```
image: ubuntu:20.04
before_script:
  - apt-get update
  - apt-get install -y ripgrep strace

test:
  script:
    - mkdir -p /tmp/test
    - cd /tmp/test
    - echo test > test
    - echo temp > temp
    - ls -la
    - grep te *
    - rg --version
    - rg te
```

#### What is the actual behavior?

https://gitlab.com/danpintara/ripgrep-test/-/pipelines/160716263

#### What is the expected behavior?

It should works, at least show something like what `grep` does.
```
$ grep te *
temp:temp
test:test
```

---

_Comment by @BurntSushi on 2020-06-27 11:50_

My guess is that gitlab is improperly advertising stdin as available. Please use `rg te ./` instead. (When you provide no paths to ripgrep to search, it has to guess between reading stdin or searching the current directory. It does the former only when stdin is available. So ripgrep is likely trying to read from it, and gitlab is likely leaving it open but not writing anything to stdin. Which means ripgrep will block forever.)

---

_Closed by @BurntSushi on 2020-06-27 11:50_

---

_Label `invalid` added by @BurntSushi on 2020-06-27 11:50_

---

_Comment by @ghost on 2020-06-27 12:45_

Sorry for putting an invalid bug report, but thank you, @BurntSushi, for helping me with this problem, which I've struggled for nearly the whole day. I confirmed that your solution works here: https://gitlab.com/danpintara/ripgrep-test/-/jobs/614149788

To clarify, it's not waiting forever but it just quit. From what I got from your comment, the situation is somewhat like this:
```bash
$ echo | rg te | cat

$ echo $?
1
```

If I put the path, ripgrep will use that instead of using the standard input:
```bash
$ echo | rg te . | cat
./test:test
./temp:temp

$ echo $?
1
```

I learned new things today.

---

_Comment by @BurntSushi on 2020-06-27 12:49_

Ah okay. It probably quit because stdin was closed.

---
