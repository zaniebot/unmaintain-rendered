```yaml
number: 901
title: Is --ignore-file having lower precedence than .gitignore intentional?
type: issue
state: closed
author: mqudsi
labels:
  - question
assignees: []
created_at: 2018-04-29T21:42:38Z
updated_at: 2018-04-29T22:02:56Z
url: https://github.com/BurntSushi/ripgrep/issues/901
synced_at: 2026-01-12T16:13:22Z
```

# Is --ignore-file having lower precedence than .gitignore intentional?

---

_@mqudsi_

#### What version of ripgrep are you using?

rg 0.8.1

#### What operating system are you using ripgrep on?

Linux/WSL

#### Describe your question, feature request, or bug.

I'm curious as to whether the intention to assign ignore files passed in via `--ignore-file` a _lower_ precedence than an existing `.gitignore` (or other vcs ignore). This means the only way to exclude a file already included in `.gitignore` is to use a hardcoded `.rgignore`/`.ignore` file or exclude .gitignore entirely with `--no-vcs-ignore`.

#### If this is a bug, what are the steps to reproduce the behavior?

```fish
mqudsi@ZBook /m/c/U/M/g/r/j/foo> ls
mqudsi@ZBook /m/c/U/M/g/r/j/foo> mkdir bar
mqudsi@ZBook /m/c/U/M/g/r/j/foo> touch bar/baz
mqudsi@ZBook /m/c/U/M/g/r/j/foo> echo "bar" > .gitignore
mqudsi@ZBook /m/c/U/M/g/r/j/foo> echo "!bar" > exclude.txt
mqudsi@ZBook /m/c/U/M/g/r/j/foo> rg --ignore-file exclude.txt --files
mqudsi@ZBook /m/c/U/M/g/r/j/foo> mv exclude.txt .ignore
mqudsi@ZBook /m/c/U/M/g/r/j/foo> rg --files
bar/baz
mqudsi@ZBook /m/c/U/M/g/r/j/foo>
```

#### Rationale

Presume an existing directory tree with valid `.gitignore`, `.rgignore`, and `.ignore` files, and a need to specify a custom `rg --files` list (for example, to generate a list of files that should be included in a as `Makefile` dependencies dynamically), which has no bearing on whether or not the user wants to include these files in `fd` or `rg` output.

Additionally, consider the case of a 3rd party tool that wishes to exclude certain files from output and generates its output with `rg`. The only way to force a particular file/directory to be included without allowing all other ignored entities would be to back up a `.ignore` file (if it exists), and use that filename for its own content, pass in the backuped version of `.ignore` as `--ignore-file` (and risk running into this same bug!), run `rg`, then restore the `.ignore` (ok, I'm being dramatic).

Really, it's just the idea that anything manually specified for *this* run should take precedence over a default ignore, the same reason why `.rgignore` has priority over `.gitignore` (command-specific, then tool-specific, then repository-specific excludes given priority in that order).

But if there's a good reason why this _isn't_ currently the case, I apologize and would ask enlightenment.

Thanks!


---

_Comment by @BurntSushi on 2018-04-29 21:49_

Yes, it is intended. The point of `--ignore-file` is to specify a "global" set of ignore rules. The `.gitignore`/`.rgignore`/`.ignore` files are all more specific and apply to a particular directory tree, which is interpreted as overriding the global rules.

If you need to specify rules for a specific run of ripgrep, then use the `-g/--glob` flag.

---

_Closed by @BurntSushi on 2018-04-29 21:49_

---

_Label `question` added by @BurntSushi on 2018-04-29 21:49_

---

_Comment by @mqudsi on 2018-04-29 22:02_

Thanks.

---
