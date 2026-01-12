```yaml
number: 1948
title: Issue with global ignore at ~/.config location
type: issue
state: closed
author: daedroza
labels: []
assignees: []
created_at: 2021-07-25T10:34:42Z
updated_at: 2021-07-25T14:14:46Z
url: https://github.com/BurntSushi/ripgrep/issues/1948
synced_at: 2026-01-12T16:13:24Z
```

# Issue with global ignore at ~/.config location

---

_@daedroza_

#### What version of ripgrep are you using?

13.0.0

#### How did you install ripgrep?

Locally built binary.

#### What operating system are you using ripgrep on?

Ubuntu 20.04

#### Describe your bug.

Global ignore file located either at ~/.config/rg and ~/.config/ripgrep is not working.

#### What are the steps to reproduce the behavior?
Create a ignore text file and soft link it to ~/.config/rg/ignore, ~/.config/rg/.ignore, ~/.config/ripgrep/ignore and ~/.config/ripgrep/.ignore.
Now run rg command anywhere, should respect the ignore file.

#### What is the actual behavior?
rg doesn't respect the ignore file.

#### What is the expected behavior?
rg should respect the ignore file similar to using --ignore-file.


---

_Comment by @BurntSushi on 2021-07-25 10:40_

This ticket isn't actionable because it contains no reproduction steps. Please fill out the complete issue template.

---

_Comment by @daedroza on 2021-07-25 11:32_

@BurntSushi : I've updated the ticket from template, please let me know if I need to do anything else.

---

_Comment by @BurntSushi on 2021-07-25 11:41_

None of the things you've done are supported by ripgrep. So I don't know why you expect them to do anything. If you want a config file, follow the steps in the docs about setting RIPGREP_CONFIG_PATH.

---

_Closed by @BurntSushi on 2021-07-25 11:41_

---

_Comment by @daedroza on 2021-07-25 11:46_

Ah, okay, I just saw this https://github.com/BurntSushi/ripgrep/commit/cc90511ab29d3c37e873acac0fd80f90473f54bb#diff-0ec86f4f4d491d0c2cd77e264ed6337b7cb0d064b0280281029e180f009b4799R8 and hence expected that it worked so I could have a single ignore for all use case.

---

_Comment by @BurntSushi on 2021-07-25 13:49_

That's from 2016. And it's library level docs just giving an example, e.g., finding an ignore file in a parent directory.

---

_Comment by @daedroza on 2021-07-25 14:14_

True however I didn't knew there would such a large refactor in the code base and the way it was supposed to be used globally. I read the related issues and the controversial "~" not supported in the RIPGREP_CONFIG_PATH file (even though a workaround was suggested using alias, but didn't work with my vim settings).

Sorting through all the things, I have managed to produce what I initially wanted to do using the CONFIG_PATH. I just didn't thought of using --ignore-file through CONFIG_PATH file before and hence created this ticket. Stupid of me, lol.

As always, thanks for your time and for replying so quickly! @BurntSushi 


---
