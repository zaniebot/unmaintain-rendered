```yaml
number: 2419
title: "--ignore-file <file> is not processed correctly in rg config file"
type: issue
state: closed
author: rieje
labels:
  - invalid
assignees: []
created_at: 2023-02-16T15:55:37Z
updated_at: 2023-02-16T17:00:57Z
url: https://github.com/BurntSushi/ripgrep/issues/2419
synced_at: 2026-01-12T16:13:24Z
```

# --ignore-file <file> is not processed correctly in rg config file

---

_@rieje_

#### What version of ripgrep are you using?
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
#### How did you install ripgrep?
Arch Linux official repositories
#### What operating system are you using ripgrep on?
Arch Linux
#### Describe your bug.
--ignore-file ~/.config/rg/general.rgignore in rg config file does not work:

> error: Found argument '--ignore-file ~/.config/rg/general.rgignore' which wasn't expected, or isn't valid in this context
	Did you mean --ignore-file-case-insensitive?

Works when specified in the command line.

#### What are the steps to reproduce the behavior?

Add `--ignore-file ~/.config/rg/general.rgignore` to config file. 

    $ rg hey .

#### What is the actual behavior?

Show the command you ran and the actual output. Include the `--debug` flag in
your invocation of ripgrep.

    $ rg hey . --debug

    DEBUG|rg::config|crates/core/config.rs:40: /home/gcheng/.config/rg/config: arguments loaded from config file: ["--ignore-file ~/.config/rg/general.rgignore"]
    DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--ignore-file ~/.config/rg/general.rgignore", "hey", ".", "--debug"]
    error: Found argument '--ignore-file ~/.config/rg/general.rgignore' which wasn't expected, or isn't valid in this context
      Did you mean --ignore-file-case-insensitive?

    USAGE:

        rg [OPTIONS] PATTERN [PATH ...]
        rg [OPTIONS] -e PATTERN ... [PATH ...]
        rg [OPTIONS] -f PATTERNFILE ... [PATH ...]
        rg [OPTIONS] --files [PATH ...]
        rg [OPTIONS] --type-list
        command | rg [OPTIONS] PATTERN
        rg [OPTIONS] --help
        rg [OPTIONS] --version

    For more information try --help

#### What do you think ripgrep should have done?

Process the argument correctly.


---

_Comment by @BurntSushi on 2023-02-16 16:05_

As explained in the man page [and the GUIDE](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file), every single line in a config file is its own argument. So when you do `--ignore-file foo`, ripgrep sees that as one single argument. You can fix it by either joining them with a `=`, i.e., `--ignore-file=foo`, or by putting each on their own line:

```
--ignore-file
foo
```

---

_Closed by @BurntSushi on 2023-02-16 16:05_

---

_Label `invalid` added by @BurntSushi on 2023-02-16 16:05_

---

_Comment by @rieje on 2023-02-16 17:00_

Oh whoops, thanks. I found shell stuff like `~` and `$XDG_CONFIG_HOME` don't work in the config.

---
