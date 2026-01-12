```yaml
number: 191
title: "~/.gitignore with 'tags' does not work"
type: issue
state: closed
author: sumonto
labels:
  - question
assignees: []
created_at: 2016-10-21T20:47:06Z
updated_at: 2016-10-21T22:04:33Z
url: https://github.com/BurntSushi/ripgrep/issues/191
synced_at: 2026-01-12T18:23:11Z
```

# ~/.gitignore with 'tags' does not work

---

_@sumonto_

~/.gitignore with 'tags' does not work


---

_Comment by @BurntSushi on 2016-10-21 20:49_

This ticket doesn't have enough detail for me to reproduce your problem. Could you please provide a minimal reproducible example?

For example, this works:

```
[andrew@Cheetah 191] echo foo > tags
[andrew@Cheetah 191] rg foo
tags
1:foo
[andrew@Cheetah 191] echo tags > .gitignore
[andrew@Cheetah 191] rg foo
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```


---

_Label `question` added by @BurntSushi on 2016-10-21 20:50_

---

_Comment by @sumonto on 2016-10-21 21:04_

OS: Mac OSx

```
rg --version
0.2.1

$ cat ~/createtags.sh
#!/bin/bash
echo "Creating ctags files ..."
ctags -R .

------------------------------------------------------
cat ~/.gitconfig
....
[core]
    excludesfile = /Users/Sumonto/.gitignore_global
    excludesfile = /Users/Sumonto/.gitignore
....

------------------------------------------------------
$ cat ~/.gitignore
# List of excluded files
tags
------------------------------------------------------

Search for any function name
rg -p getPath

The tags file is listed and one the files.



..
```


---

_Comment by @BurntSushi on 2016-10-21 21:09_

It's really hard for me to understand your reproduction. It sounds like what you're telling me is that your _global_ `.gitignore` file (as specified in your `~/.gitconfig`, namely, `~/.gitignore` isn't intrinsically meaningful) isn't being respected, but without more information about which directories you're running `rg` in, it's impossible to tell what your actual problem is.

If you're saying that your global `.gitignore` isn't respected, then that's because ripgrep doesn't support it yet, which would make this a dupe of #9. (I'm in the process of working on ironing out a variety of ignore related problems/features, and this is one of them.)


---

_Comment by @sumonto on 2016-10-21 21:30_

Ok, I thought ~/.gitignore is supported? (.gitignore is in the home directory)
The tags file is on the root folder of a git clone.
So how do I ignore the 'tags' file?

thanks.


---

_Comment by @BurntSushi on 2016-10-21 22:04_

> Ok, I thought ~/.gitignore is supported?

Please see #9, which is about adding support for **global gitignore configuration**. Your `~/.gitignore` isn't automatically used by `git`. You had to tell it to use it in your `~/.gitconfig`. Please see `man gitignore` for the details.

> So how do I ignore the 'tags' file?

Put it in an `.ignore` file that is in a parent directory of your clones (presumably, `~/.ignore` would work).

When #9 is done, then your current setup should work out of the box.


---

_Closed by @BurntSushi on 2016-10-21 22:04_

---
