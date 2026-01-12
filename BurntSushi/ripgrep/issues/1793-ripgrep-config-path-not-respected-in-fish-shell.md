```yaml
number: 1793
title: $RIPGREP_CONFIG_PATH not respected in fish shell
type: issue
state: closed
author: bronzehedwick
labels:
  - invalid
assignees: []
created_at: 2021-02-03T14:31:41Z
updated_at: 2021-03-14T03:12:00Z
url: https://github.com/BurntSushi/ripgrep/issues/1793
synced_at: 2026-01-12T16:13:24Z
```

# $RIPGREP_CONFIG_PATH not respected in fish shell

---

_@bronzehedwick_

#### What version of ripgrep are you using?

ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Homebrew

#### What operating system are you using ripgrep on?

Darwin lpub0055 18.7.0 Darwin Kernel Version 18.7.0: Tue Nov 10 00:07:31 PST 2020; root:xnu-4903.278.51~1/RELEASE_X86_64 x86_64

#### Describe your bug.

Setting the `$RIPGREP_CONFIG_PATH` environment variable to my config file does not result in my config file being respected in the fish shell. Was able to see the behavior fine in bash.

#### What are the steps to reproduce the behavior?

- `set -U RIPGREP_CONFIG_PATH ~/.config/ripgrep/ripgreprc`
- Add option to config file: `--type-add jsx:*.jsx*`

#### What is the actual behavior?

`rg -tjsx console` outputs:

```
unrecognized file type: jsx
```

#### What is the expected behavior?
Read the configuration file.


---

_Comment by @BurntSushi on 2021-02-03 14:36_

ripgrep doesn't care about your shell. All it does is read the environment variable in the standard way. If it isn't there then it isn't set.

Consider using the `--debug` flag. It will tell you if a config file is being loaded, and if so, the additional flags that come from it.

---

_Comment by @bronzehedwick on 2021-02-03 15:04_

Hmm interesting. I might not have a working `rg`, then, because it doesn't have the `--debug` flag.

```
$ rg --debug -tjsx console
fish: Unknown command: rg --debug
```

---

_Comment by @BurntSushi on 2021-02-03 15:09_

Please read the error. The error doesn't say that the `--debug` flag doesn't exist. The error is *from your shell* and is, for whatever reason, telling you that the command `rg --debug` does not exist. I have no idea why Fish is interpreting `rg --debug` as a single command, but it's definitely not a problem with ripgrep.

I don't use Fish so I can't really be of any more help. My guess is that there is something borked about your Fish configuration. So I would recommend trying vanilla Fish. If that works, then add your configuration back piece by piece until you find the thing that breaks stuff. In short, debug the problem.

---

_Closed by @BurntSushi on 2021-02-03 15:09_

---

_Label `invalid` added by @BurntSushi on 2021-02-03 15:09_

---

_Comment by @bronzehedwick on 2021-02-03 15:18_

Okay thanks.

---

_Comment by @ilyagr on 2021-03-14 03:12_

@bronzehedwick The correct way to set this up is `set -Ux RIPGREP_CONFIG_PATH ~/.config/ripgrep/ripgreprc`, I believe (note the extra `x`).

---
