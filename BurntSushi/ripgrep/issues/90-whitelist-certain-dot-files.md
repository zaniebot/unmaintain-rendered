```yaml
number: 90
title: Whitelist certain dot files?
type: issue
state: closed
author: kaushalmodi
labels:
  - bug
assignees: []
created_at: 2016-09-25T20:49:38Z
updated_at: 2016-11-07T16:56:33Z
url: https://github.com/BurntSushi/ripgrep/issues/90
synced_at: 2026-01-12T18:23:11Z
```

# Whitelist certain dot files?

---

_@kaushalmodi_

Hello,

I am trying to whitelist certain dot files. But it does not seem to work.

Is the syntax in `.ignore` file incorrect?

Thanks.

```
~/temp/:proj/SOMEPRJ> tree -a
.
├── .file
└── .ignore

0 directories, 2 files
km²~/temp/:proj/SOMEPRJ> cat .ignore
!.file
km²~/temp/:proj/SOMEPRJ> cat .file
foo
km²~/temp/:proj/SOMEPRJ> which rg
rg:      aliased to \rg --follow --no-mmap --no-ignore-vcs --smart-case
km²~/temp/:proj/SOMEPRJ> rg foo
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```


---

_Comment by @kaushalmodi on 2016-09-25 20:51_

Here with `--debug`:

```
km²~/temp/:proj/SOMEPRJ> rg foo --debug
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'f',
        'o',
        'o'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(foo)], limit_size: 250, limit_class: 10 }
DEBUG:rg::ignore: ./.ignore ignored because it is hidden
DEBUG:rg::ignore: ./.file ignored because it is hidden
IO error for operation on ./.#.ignore: No such file or directory (os error 2)
DEBUG:rg::ignore: kmodi/temp/proj/./#.ignore# ignored by Pattern { from: "/home/kmodi/.ignore", original: "*#*#", pat: "**/*#*#", whitelist: false, only_dir: false }
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```


---

_Comment by @BurntSushi on 2016-09-25 20:55_

Are you using current master? If not, you need to use `.rgignore`. Current master supports both `.rgignore` and `.ignore`, with the intent of deprecating `.rgignore`.

With that said, actually, you can't whitelist hidden files. The only way a hidden file gets searched is if `--hidden` is set or if `-g` is used to explicitly match a hidden file.

This does seem like a shortcoming. 


---

_Label `bug` added by @BurntSushi on 2016-09-25 20:55_

---

_Comment by @BurntSushi on 2016-09-25 20:57_

I think what this means is that we should do the hidden check _last_, which will enable CLI globs, ignores or file types the opportunity to whitelist them.


---

_Closed by @BurntSushi on 2016-09-25 22:31_

---

_Comment by @kaushalmodi on 2016-09-26 01:34_

> Are you using current master? 

Yes, I am, and thus the reference to `.ignore` :)

> This does seem like a shortcoming.

Thanks for recognizing this as a bug and fixing it this quick!


---

_Comment by @ye on 2016-11-04 20:32_

We use GitLab CI and its configuration file is named `.gitlab-ci.yml` and because of `rg` skips hidden files, none of the content of hidden files were searched, including `.gitlab-ci.yml`.

``` bash
$ cat <<EOF > .hidden-file
> docker pull python:3.4-wheezy
> EOF
$ ll .hidden-file 
-rw-r--r--  1 ye  staff  30 Nov  4 16:24 .hidden-file
$ cat .hidden-file 
docker pull python:3.4-wheezy
$ rg python:3.4-wheezy
$ 
$ rg --no-ignore python-3.4:wheezy 
$
$ cat .hidden-file | grep 'python-3.4:wheezy'
docker pull python-3.4:wheezy
$ cat .hidden-file | rg  'python-3.4:wheezy'
1:docker pull python-3.4:wheezy
```


---

_Comment by @BurntSushi on 2016-11-04 21:52_

@ye As I see it, _today_, you have two options available to you. First, pass the `--hidden` flag, which will search hidden files and directories. Second, add your hidden yml files to a whitelist in your `.ignore` file.

I think I prefer you create a new issue, since the specific bug in this ticket has been fixed. In this new issue, could you explain why neither of the aforementioned options work for you?


---

_Comment by @ye on 2016-11-07 16:38_

@BurntSushi thank you. `--hidden` flag worked. `.ignore` file didn't. Thanks for the help!

Some project owners preferred less issues being opened and always searching for existing issues and tack on rather than open a new ticket. I wasn't sure, so I commented here. 


---

_Comment by @BurntSushi on 2016-11-07 16:51_

@ye if you'd be willing, i would definitely appreciate a big report on how .ignore didn't work for you.


---

_Comment by @ye on 2016-11-07 16:56_

@BurntSushi Ok, will do. 


---
