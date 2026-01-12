```yaml
number: 1548
title: Relative paths in ripgreprc ($RIPGREP_CONFIG_PATH) ?
type: issue
state: closed
author: luckman212
labels:
  - question
assignees: []
created_at: 2020-04-10T20:40:59Z
updated_at: 2020-04-10T23:27:41Z
url: https://github.com/BurntSushi/ripgrep/issues/1548
synced_at: 2026-01-12T16:13:23Z
```

# Relative paths in ripgreprc ($RIPGREP_CONFIG_PATH) ?

---

_@luckman212_

#### What version of ripgrep are you using?
```
$ rg --version
ripgrep 12.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?
Homebrew

#### What operating system are you using ripgrep on?
macOS 10.15.4

#### Describe your question, feature request, or bug.
I am exporting **RIPGREP_CONFIG_PATH** in my `.bash_profile` to have some global options applied. Works fine!
```
export RIPGREP_CONFIG_PATH=$HOME/path/to/my/ripgreprc
```
My question is: is it possible to use relative paths somehow to source `--ignore-file` directives from the same directory as the ripgreprc? It would make maintaining the ripgreprc easier and more portable. As it stands now, seems I have to fill out the explicit path e.g.
```
--ignore-file=/Users/luke/foo/bar/baz/quux.ignore
```
Variables such as `$HOME` don't seem to work here either... sorry if this is a dumb question (& thank you ❤️ for ripgrep!!)

---

_Comment by @BurntSushi on 2020-04-10 21:26_

There is no way, no, by design. Sorry. This is a trade off I made in the configuration file format. Namely, the correspondence between the file's contents and what actually gets passed to ripgrep's command line is very clear precisely because there is no escaping or quoting or other things. Some form of escaping would need to be introduced in order to support environment variables like this.

You have a few options available to you:

* Use aliases instead of configuration files. Aliases can use env vars or whatever else.
* Similarly, write a wrapper script.
* Use a config file with absolute paths. If you have multiple environments with different paths, then create one config file for each. Then set `RIPGREP_CONFIG_PATH` appropriate on each of those systems.

---

_Closed by @BurntSushi on 2020-04-10 21:26_

---

_Label `question` added by @BurntSushi on 2020-04-10 21:26_

---

_Comment by @luckman212 on 2020-04-10 23:27_

@BurntSushi Ok, I understand, thank you for the reply and the amazingly useful tool.

---
