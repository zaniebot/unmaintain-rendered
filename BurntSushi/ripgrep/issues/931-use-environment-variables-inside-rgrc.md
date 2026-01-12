```yaml
number: 931
title: Use environment variables inside .rgrc
type: issue
state: closed
author: bastienbc
labels:
  - question
  - wontfix
assignees: []
created_at: 2018-05-29T08:10:12Z
updated_at: 2020-12-06T22:54:21Z
url: https://github.com/BurntSushi/ripgrep/issues/931
synced_at: 2026-01-12T16:13:22Z
```

# Use environment variables inside .rgrc

---

_@bastienbc_

#### What version of ripgrep are you using?

ripgrep 0.8.1 (rev c8e9f25b85)
+SIMD -AVX

#### How did you install ripgrep?

Using binary release

#### What operating system are you using ripgrep on?

Distributor ID: Ubuntu
Description:    Ubuntu 16.04.4 LTS
Release:        16.04
Codename:       xenial

#### Describe your question, feature request, or bug.

I think environment variables used inside RIPGREP_CONFIG_PATH file should be expanded.

I have a file in my home (.rgrc):

```
--smart-case
--hidden
--ignore-file
$HOME/.rgignore
```

But when I use `rg` I have this message:
```
$ export RIPGREP_CONFIG_PATH=$HOME/.rgrc
$ rg any > /dev/null
$HOME/.rgignore: No such file or directory (os error 2)
```

I would like to be able to reuse those files (.rgrc and .rgingnore) across multiple accounts, this I source my zsh configuration from a git repo.

Do you think it would be possible ?


---

_Comment by @BurntSushi on 2018-05-29 12:44_

No, sorry.

ripgrep's configuration file is a thin veneer over specifying the argv sent to ripgrep's process, at which point, the argv parser merges all of the configuration directly. There are only two simple rules to follow: each line is a single argv parameter (after trimming ASCII whitespace) and lines beginning with a `#` are ignored. Adding environment variable interpolation means changing those rules to something quite a bit more complex, which includes the ability to escape environment variables so that folks can use them literally as well. Moreover, this sounds like a feature that begets even more features. Do we also need to support the `${...}` syntax? What about `${name:-default}` syntax?

You have many alternative choices available to you:

* Use two different config files on your machines with different home directories.
* Add your config that uses environment variables to an alias wrapper around `rg`.
* Add your config that uses environment variables to a shell script wrapper around `rg`.

---

_Closed by @BurntSushi on 2018-05-29 12:44_

---

_Label `question` added by @BurntSushi on 2018-05-29 12:44_

---

_Label `wontfix` added by @BurntSushi on 2018-05-29 12:44_

---

_Comment by @okdana on 2018-05-29 13:28_

Here's a zsh script you could use if you were committed to the idea:

```sh
 #!/usr/bin/env zsh

rgrc=${RIPGREP_CONFIG_PATH:-$HOME/.rgrc}

if [[ -e $rgrc ]]; then
  trap 'rm -f -- $tmp' EXIT INT
  tmp=$( mktemp )
  print -r - ${(Xe)"$( < $rgrc )"} > $tmp
  rgrc=$tmp
else
  rgrc=
fi

RIPGREP_CONFIG_PATH=$rgrc rg "$@"
```

It takes your config file and runs it through zsh's `(e)` expansion flag (which performs parameter expansion and command substitution on the input), then dumps the evaluated version to a temp file, and passes that as the config file to `rg`.

Obviously some security concerns here, since the script can execute arbitrary commands in the config file. It also adds probably 2 to 5 ms of over-head to each `rg` call. You could make it faster by caching the evaluated result. Not sure about making it safer, i don't think zsh has an option to do parameter expansion without command substitution.

---

_Comment by @bastienbc on 2018-05-29 17:48_

Thanks for your reply.

I was hoping that something like https://crates.io/crates/shellexpand could be used to do the job, but it introduces a regression for people with $ in their configuration as you pointed out.

 I will do it differently then.



---

_Comment by @sudoforge on 2020-12-06 22:54_

Came across this because I was surprised it wasn't supported (and @BurntSushi isn't interested in supporting it). @okdana's solution is great for general purpose, however, in my case the only thing I need to reference is a file in my home directory. With that in mind, an alias is a much more efficient way to solve this problem:

```
alias rg="\rg --ignore-file ~/.ignore"
```

The `\` is a shell "trick" that ignores aliases. You could alternatively specify `/usr/bin/rg` or whatever.

---
