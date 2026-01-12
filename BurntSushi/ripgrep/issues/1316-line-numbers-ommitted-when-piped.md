```yaml
number: 1316
title: Line numbers ommitted when piped
type: issue
state: closed
author: geowa4
labels: []
assignees: []
created_at: 2019-07-01T12:29:53Z
updated_at: 2019-07-01T13:44:11Z
url: https://github.com/BurntSushi/ripgrep/issues/1316
synced_at: 2026-01-12T16:13:23Z
```

# Line numbers ommitted when piped

---

_@geowa4_

#### What version of ripgrep are you using?

11.0.1

#### How did you install ripgrep?

Homebrew

#### What operating system are you using ripgrep on?

macOS Mojave

#### Describe your question, feature request, or *bug*.

Line numbers of matches do not appear in piped output.

#### If this is a bug, what are the steps to reproduce the behavior?

This is my attempt to search for all files referencing "playbook.yml" in an Ansible role created with Molecule (https://molecule.readthedocs.io/en/stable/).
```bash
$ rg playbook.yml
README.md:30:[playbook.yml](./molecule/default/playbook.yml)

$ rg playbook.yml | xargs echo
README.md:[playbook.yml](./molecule/default/playbook.yml)
```

This is my ripgreprc.
```
--no-heading
--max-columns=120
--glob=!git/*
--smart-case

--type-add
web:*.{html,css,js}*
```



---

_Comment by @BurntSushi on 2019-07-01 13:02_

I don't think I understand why you're reporting this as an issue. This is intended and correct behavior. By default, piping output would also disable the human-friendly heading output, but you've explicitly disabled that.

---

_Closed by @BurntSushi on 2019-07-01 13:02_

---

_Comment by @geowa4 on 2019-07-01 13:44_

Sorry for making a report when I should have RTFM more thoroughly. I have found `--line-number`, which forces the line number to appear in piped output. 

---
