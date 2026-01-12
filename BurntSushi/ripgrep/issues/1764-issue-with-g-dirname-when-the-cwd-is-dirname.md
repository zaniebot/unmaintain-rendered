```yaml
number: 1764
title: "Issue with -g \"!dirname/\" when the CWD is \"dirname\""
type: issue
state: open
author: twmr
labels:
  - bug
assignees: []
created_at: 2020-12-19T11:43:21Z
updated_at: 2021-06-01T00:12:06Z
url: https://github.com/BurntSushi/ripgrep/issues/1764
synced_at: 2026-01-12T16:13:24Z
```

# Issue with -g "!dirname/" when the CWD is "dirname"

---

_@twmr_

#### What version of ripgrep are you using?

```
ripgrep 12.1.1 (rev 7cb211378a)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`sudo dpkg -i ripgrep_12*.deb `

#### What operating system are you using ripgrep on?

`Ubuntu 19.10`

#### Describe your bug.

If a glob pattern and a root directory is passed to `rg`, the ignored files depend on the current working directory.

#### Why do I care?

I'm using an emacs pkg called `dumb-jump`, that executes `rg` in the directory of the currently visited file and not in the project root. The project root is always passed as the last parameter to `rg`. `dumb-jump` allows to pass additional parameter to ripgrep, which I need because I need to ignore large directories that make ripgrep slow.

#### What are the steps to reproduce the behavior?


```bash
#!/bin/bash
set -x

rm -rf /tmp/foo

mkdir -p /tmp/foo/to-be-ignored
mkdir /tmp/foo/dir1

touch /tmp/foo/to-be-ignored/a
touch /tmp/foo/dir1/d

#### setup done ####

cd /tmp/foo/to-be-ignored
# no output expected:  BUG
rg --files -g '!to-be-ignored/' /tmp/foo | grep to-be-ignored

cd /tmp/foo/dir1
# no output expected:  OK
rg --files -g '!to-be-ignored/' /tmp/foo | grep to-be-ignored

cd /tmp/foo
# no output expected:  OK
rg --files -g '!to-be-ignored/' /tmp/foo | grep to-be-ignored


cd /tmp/foo/to-be-ignored
# no output expected:  OK
rg --files -g '!dir1/' /tmp/foo | grep dir1

cd /tmp/foo/dir1
# no output expected:  BUG
rg --files -g '!dir1/' /tmp/foo | grep dir1

cd /tmp/foo
# no output expected:  OK
rg --files -g '!dir1/' /tmp/foo | grep dir1
```

#### What is the actual behavior?

```
+ mkdir -p /tmp/foo/to-be-ignored
+ mkdir /tmp/foo/dir1
+ touch /tmp/foo/to-be-ignored/a
+ touch /tmp/foo/dir1/d
+ cd /tmp/foo/to-be-ignored
+ rg --files -g '!to-be-ignored/' /tmp/foo
+ grep to-be-ignored
/tmp/foo/to-be-ignored/a
+ cd /tmp/foo/dir1
+ rg --files -g '!to-be-ignored/' /tmp/foo
+ grep to-be-ignored
+ cd /tmp/foo
+ rg --files -g '!to-be-ignored/' /tmp/foo
+ grep to-be-ignored
+ cd /tmp/foo/to-be-ignored
+ grep dir1
+ rg --files -g '!dir1/' /tmp/foo
+ cd /tmp/foo/dir1
+ grep dir1
+ rg --files -g '!dir1/' /tmp/foo
/tmp/foo/dir1/d
+ cd /tmp/foo
+ rg --files -g '!dir1/' /tmp/foo
+ grep dir1
```

If the current-working directory is the same as the ignored directory passed to `-g`, then the glob pattern doesn't work.

#### What is the expected behavior?

In all the `rg` calls in the above bash script, I don't expect any output. 


---

_Label `bug` added by @BurntSushi on 2021-06-01 00:12_

---
