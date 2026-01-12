```yaml
number: 1468
title: ignores config file on Mac
type: issue
state: closed
author: jadamsvac
labels:
  - invalid
assignees: []
created_at: 2020-01-22T18:02:47Z
updated_at: 2020-01-22T18:08:16Z
url: https://github.com/BurntSushi/ripgrep/issues/1468
synced_at: 2026-01-12T16:13:23Z
```

# ignores config file on Mac

---

_@jadamsvac_

#### What version of ripgrep are you using?

Replace this text with the output of `rg --version`.

#### How did you install ripgrep?

brew, but the same error is there when installed through cargo.

#### What operating system are you using ripgrep on?

Macos Catalina 10.16.2

#### Describe your question, feature request, or bug.

It does not read the config file, as shown by the following two commands.
❯ cat $RIPGREP_CONFIG_PATH                                                                                                                 
--type-add web:*.{html,css,js}*
❯ rg --debug -tweb blah                                                                                                                    
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:59: literal prefixes detected: Literals { lits: [Complete(blah)], limit_size: 250, limit_class: 10 }
unrecognized file type: web

---

_Comment by @BurntSushi on 2020-01-22 18:08_

This behavior is correct as far as I can tell. Please consult the [guide's section on config file syntax](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file). Namely, you have `--type-add web:.{html,css,js}`, but every line in the config file is a _single_ shell argument. You either need

```
--type-add=web:*.{html,css,js}
```

or

```
--type-add
web:*.{html,css,js}
```

In the future, please also consider using code blocks when showing commands in GitHub issues.

---

_Closed by @BurntSushi on 2020-01-22 18:08_

---

_Label `invalid` added by @BurntSushi on 2020-01-22 18:08_

---
