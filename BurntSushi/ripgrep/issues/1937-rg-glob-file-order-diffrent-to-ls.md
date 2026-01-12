```yaml
number: 1937
title: rg glob file order diffrent to ls
type: issue
state: closed
author: redaready
labels:
  - invalid
assignees: []
created_at: 2021-07-16T08:51:36Z
updated_at: 2021-07-16T10:30:44Z
url: https://github.com/BurntSushi/ripgrep/issues/1937
synced_at: 2026-01-12T16:13:24Z
```

# rg glob file order diffrent to ls

---

_@redaready_

#### What version of ripgrep are you using?

12.1.1

#### How did you install ripgrep?

Github binary releases.

#### What operating system are you using ripgrep on?

Amazon Linux 2

#### Describe your bug.

Give a high level description of the bug.

#### What are the steps to reproduce the behavior?
 rg *.log --files
20210716-000000.log
20210715-000000.log
20210714-000000.log
20210713-000000.log
20210712-000000.log
20210711-000000.log
20210710-000000.log
20210709-000000.log
20210708-000000.log
20210707-000000.log
20210706-000000.log

but with ls or grep we have 
ls -alh *.log
20210706-000000.log
20210707-000000.log
20210708-000000.log
20210709-000000.log
20210710-000000.log
20210711-000000.log
20210712-000000.log
20210713-000000.log
20210714-000000.log
20210715-000000.log
20210716-000000.log

What do you think ripgrep should have done?

rg should have some parameter to config the order of files


---

_Comment by @redaready on 2021-07-16 08:56_

rg *.log --files --sortr path ?

---

_Comment by @BurntSushi on 2021-07-16 10:30_

Yes, you want `--sort path` if you want to sort files in ascending order lexicographically. And `--sortr path` to do it in reverse. See the docs for other values you can pass to `--sort`.

Note that using `--sort` disables parallelism.

---

_Closed by @BurntSushi on 2021-07-16 10:30_

---

_Label `invalid` added by @BurntSushi on 2021-07-16 10:30_

---
