```yaml
number: 1626
title: Environment variable for specifying path to ignore file
type: issue
state: closed
author: davidtwco
labels:
  - question
assignees: []
created_at: 2020-06-23T12:45:42Z
updated_at: 2020-06-23T13:23:11Z
url: https://github.com/BurntSushi/ripgrep/issues/1626
synced_at: 2026-01-12T16:13:23Z
```

# Environment variable for specifying path to ignore file

---

_@davidtwco_

#### Describe your feature request
It would be great if ripgrep could read `$RIPGREP_IGNORE_FILE` (or some other environment variable) for a path to the ignore file, falling back to the current `.ignore`, `.rgignore`, etc. locations if the variable is unset. 

This would be beneficial for users of [Nix development shells](https://nixos.org/) to be able to define their ignore file as part of their development shell and have ripgrep use it without any further manual intervention (e.g. symlinking the path of `RIPGREP_IGNORE_FILE` to `.ignore` whenever you change the development shell).

For example, when working on rustc, I have a `.ignore` file with directories like `src/llvm-project` that I don't typically work on - I define this file in [my development shell](https://github.com/davidtwco/veritas/blob/59d275b02eb40ba82d0f0298f37bdf591435175f/nix/shells/rustc.nix#L158-L181) alongside all the other stuff, and I update the symlink to that file in the working directory whenever the file changes - if I didn't have to do that, and could just enter the development shell and always have ripgrep use the most recent version of the ignore file, then that'd be great.

Thanks!

---

_Comment by @BurntSushi on 2020-06-23 12:59_

> It would be great if ripgrep could read `$RIPGREP_IGNORE_FILE` (or some other environment variable) for a path to the ignore file, falling back to the current `.ignore`, `.rgignore`, etc. locations if the variable is unset.

To clarify, are you suggesting that if `RIPGREP_IGNORE_FILE` is set, then ripgrep _shouldn't_ read any `.ignore` or `.rgignore` files?

> For example, when working on rustc, I have a `.ignore` file with directories like `src/llvm-project` that I don't typically work on - I define this file in [my development shell](https://github.com/davidtwco/veritas/blob/59d275b02eb40ba82d0f0298f37bdf591435175f/nix/shells/rustc.nix#L158-L181) alongside all the other stuff, and I update the symlink to that file in the working directory whenever the file changes - if I didn't have to do that, and could just enter the development shell and always have ripgrep use the most recent version of the ignore file, then that'd be great.

Could you say why you can't achieve this with ripgrep's `--ignore-file` flag? You can use an alias, a wrapper script or ripgrep's config file to set the flag.

---

_Label `question` added by @BurntSushi on 2020-06-23 12:59_

---

_Comment by @davidtwco on 2020-06-23 13:17_

> To clarify, are you suggesting that if `RIPGREP_IGNORE_FILE` is set, then ripgrep _shouldn't_ read any `.ignore` or `.rgignore` files?

I suppose I don't actually have a strong preference regarding this - so long as the environment variable was read.

> Could you say why you can't achieve this with ripgrep's --ignore-file flag? You can use an alias, a wrapper script or ripgrep's config file to set the flag.

I could do something like this. Given that I would only want the ignore file to take effect when I enter the shell, making a wrapper script would probably be the cleanest solution which isn't too difficult with a Nix development shell. I do think it would be nicer if I could just expose an environment variable but if you'd rather not have that, then we can close this issue :slightly_smiling_face:.

---

_Comment by @BurntSushi on 2020-06-23 13:23_

Aye. Yes, I'd rather not add another environment variable. I'm not opposed to it on principal, but I would want it to be well motivated. In particular, environment variables and tools like ripgrep don't particularly mesh well, since you might try using it inside of scripts which may then behave differently in the presence of environment variables. This is why for example GNU grep has deprecated its `GREP_OPTIONS` environment variable and is why ripgrep has a `--no-config` flag (which explicitly prevents ripgrep from respecting the `RIPGREP_CONFIG_PATH` environment variable). Overall, a careful weighing of the pros and cons led to adding `RIPGREP_CONFIG_PATH`, and that process would need to be repeated for any new environment variables.

My general feeling is that ripgrep should respect either 0 or 1 environment variables. It's hard to ask end users to keep track of more than that.

---

_Closed by @BurntSushi on 2020-06-23 13:23_

---
