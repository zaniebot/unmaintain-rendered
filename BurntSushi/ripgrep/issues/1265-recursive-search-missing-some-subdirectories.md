```yaml
number: 1265
title: recursive search missing some subdirectories
type: issue
state: closed
author: ericclone
labels:
  - question
assignees: []
created_at: 2019-04-22T23:23:40Z
updated_at: 2019-04-23T00:00:11Z
url: https://github.com/BurntSushi/ripgrep/issues/1265
synced_at: 2026-01-12T16:13:23Z
```

# recursive search missing some subdirectories

---

_@ericclone_

#### What version of ripgrep are you using?

ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD +AVS (runtime)

#### How did you install ripgrep?

cargo install ripgrep --force

#### What operating system are you using ripgrep on?

Debian Linux

#### Describe your question, feature request, or bug.

when issued "rg sometext", the files that contains such line in deeper subdirectories, for example `sub/directory/more/directory/file`, does not show up in the match

but if I cd into `sub/directory/more/subdirectory` and then `rg sometext`, file would show up.

#### If this is a bug, what are the steps to reproduce the behavior?

It is only reproducible within some directories but not others. But within those directories, the behavior is always consistent.

#### If this is a bug, what is the actual behavior?

As described above

#### If this is a bug, what is the expected behavior?

show the files in the sub directories when querying from the top level directory.


---

_Comment by @BurntSushi on 2019-04-22 23:26_

This bug report is not actionable. Please see the [issue template](https://github.com/BurntSushi/ripgrep/issues/new) and fill in all required information. Notably, I don't see any way to reproduce the bug you're reporting _and_ you didn't include the output of running ripgrep with the `--debug` flag. The output of `--debug` should tell you _why_ ripgrep is skipping directories.

From the main description of ripgrep at the top of the README and man page:

> ripgrep is a line-oriented search tool that recursively searches your current directory for a regex pattern. **By default, ripgrep will respect your .gitignore and automatically skip hidden files/directories and binary files.** ripgrep has first class support on Windows, macOS and Linux, with binary downloads available for every release. ripgrep is similar to other popular search tools like The Silver Searcher, ack and grep.

Please also consider reading the section on [automatic filtering in the GUIDE](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#automatic-filtering).

---

_Label `question` added by @BurntSushi on 2019-04-22 23:56_

---

_Comment by @ericclone on 2019-04-23 00:00_

yeah, it is being ignored because of .gitignore. adding `--no-ignore` produced the result I am expecting. Thanks for the quick response and sorry for the noise.

---

_Closed by @ericclone on 2019-04-23 00:00_

---
