```yaml
number: 823
title: "RIPGREP_CONFIG_PATH doesn't work with symlinked file"
type: issue
state: closed
author: nicknisi
labels:
  - invalid
assignees: []
created_at: 2018-02-19T20:04:56Z
updated_at: 2019-05-16T14:20:55Z
url: https://github.com/BurntSushi/ripgrep/issues/823
synced_at: 2026-01-12T16:13:22Z
```

# RIPGREP_CONFIG_PATH doesn't work with symlinked file

---

_@nicknisi_

#### What version of ripgrep are you using?

ripgrep 0.8.0 (rev )
-SIMD -AVX

#### What operating system are you using ripgrep on?


macOS 10.13.3

#### Describe your question, feature request, or bug.

ripgrep does not appear to be able to follow symlinks for a config file.

#### If this is a bug, what are the steps to reproduce the behavior?

- Create a ripgrep config file, rgrc, for example
- symlink it into your home directory
- set `export $RIPGREP_CONFIG_PATH="$HOME/.rgrc"`
- reload terminal

#### If this is a bug, what is the actual behavior?
With `export $RIPGREP_CONFIG_PATH="$HOME/.rgrc"` set in my ~/.zshrc, I get the following error when opening a new terminal:

```
/Users/nicknisi/.zshrc:58: /Users/nicknisi/.rgrc not found
```

This file does exist, but it is a symlink.


#### If this is a bug, what is the expected behavior?

I would expect ripgrep to be able to find the symlinked config file


---

_Comment by @nicknisi on 2018-02-19 20:09_

Sorry, this doesn't appear to be a ripgrep issue. 

---

_Closed by @nicknisi on 2018-02-19 20:09_

---

_Comment by @BurntSushi on 2018-02-20 01:58_

@nicknisi Could you say what the issue was? It might help others.

---

_Label `invalid` added by @BurntSushi on 2018-02-20 12:42_

---

_Comment by @nicknisi on 2018-02-20 15:17_

This just turned out to be silliness on my part when exporting the `RIPGREP_CONFIG_PATH` variable.

```diff
 # add a config file for ripgrep
-export $RIPGREP_CONFIG_PATH="$HOME/.rgrc"
+export RIPGREP_CONFIG_PATH="$HOME/.rgrc"
```

---

_Comment by @BurntSushi on 2018-02-20 15:25_

@nicknisi Ah yeah, I've been there! :) Thanks for sharing!

---

_Comment by @neillrobson on 2019-05-16 14:20_

I'd also like to add the related issue that I experienced, which is equally silly on my part (if not more so).

When you add your environment variable, make sure that you do in fact use `export`, or else the variable will not be inherited by child processes:

```diff
# add a config file for ripgrep
-RIPGREP_CONFIG_PATH="$HOME/.rgrc"
+export RIPGREP_CONFIG_PATH="$HOME/.rgrc"
```

Again, unrelated to the ripgrep utility itself, but maybe useful for others who find this page when Googling.

---
