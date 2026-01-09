---
number: 271
title: conflicts_with not working when loading from yaml
type: issue
state: closed
author: XAMPPRocky
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2015-09-22T18:29:44Z
updated_at: 2018-08-02T03:29:45Z
url: https://github.com/clap-rs/clap/issues/271
synced_at: 2026-01-07T13:12:19-06:00
---

# conflicts_with not working when loading from yaml

---

_Issue opened by @XAMPPRocky on 2015-09-22 18:29_

Trying what was suggested in #267 I added the conflicts_with flag. This had no effect, and the clap still errors wince there is no input
### Command: `tokei -l`
### Error:

```
error: The following required arguments were not supplied:
    '<input>...'

USAGE:
    tokei <input>... --languages

For more information try --help
```
### CLI file

```
name: Tokei
version: 1.1.1
author: Aaron P. <theaaronepower@gmail.com>
about: A quick CLOC (Count Lines Of Code) tool
args:
  - exclude:
      short: e
      long: exclude
      help: Will ignore all files and directories containing the word ie --exclude node_modules
      takes_value: true
  - sort:
      short: s
      long: sort
      takes_value: true
      possible_values: [files, total, blanks, code, commments]
      help: Will sort based on a certain column ie --sort=files will sort by file count.
  - input:
      index: 1
      multiple: true
      required: true
      help: The input file(s)/directory(ies)
  - languages:
      short: l
      long: languages
      conflicts_with:
          - input
      help: prints out supported languages and their extensions
```


---

_Referenced in [clap-rs/clap#239](../../clap-rs/clap/issues/239.md) on 2015-09-22 18:35_

---

_Label `T: bug` added by @sru on 2015-09-22 18:40_

---

_Label `C: args` added by @sru on 2015-09-22 18:40_

---

_Label `C: parsing` added by @sru on 2015-09-22 18:40_

---

_Label `P2: need to have` added by @sru on 2015-09-22 18:40_

---

_Referenced in [clap-rs/clap#272](../../clap-rs/clap/pulls/272.md) on 2015-09-22 19:40_

---

_Comment by @kbknapp on 2015-09-22 19:42_

Once #272 is merged we'll update crates.io Two version in one day! :smile: haha

@Aaronepower thanks for taking the time to file these issues! :+1: 


---

_Closed by @homu on 2015-09-23 01:23_

---

_Comment by @kbknapp on 2015-09-23 02:09_

@Aaronepower v1.4.2 is on crates.io and should have fixed this bug for you.


---
