```yaml
number: 2019
title: Combining -z and -l works but prints error messages
type: issue
state: closed
author: golimarrrr
labels:
  - invalid
assignees: []
created_at: 2021-10-14T06:46:38Z
updated_at: 2023-11-24T20:13:27Z
url: https://github.com/BurntSushi/ripgrep/issues/2019
synced_at: 2026-01-12T16:13:24Z
```

# Combining -z and -l works but prints error messages

---

_@golimarrrr_

#### What version of ripgrep are you using?

ripgrep 13.0.0 (rev af6b6c543b)

#### How did you install ripgrep?

Downloaded the zip file for Windows

#### What operating system are you using ripgrep on?

Win10 1809

#### Describe your bug.

Combining -z and -l works but there is a seemingly unrelated error in the output

#### What are the steps to reproduce the behavior?

`rg -zl pattern` having gz files that contain "pattern" in the current directory

#### What is the actual behavior?

```
>c:\temp\rg -zl "Connection reset"
MyLogFile-56.log.gz:
-------------------------------------------------------------------------------
stdout: Invalid argument

gzip:
-------------------------------------------------------------------------------
MyLogFile-54.log.gz:
-------------------------------------------------------------------------------
stdout: Invalid argument

gzip:

```


#### What is the expected behavior?

```
>c:\temp\rg -zl "Connection reset"
MyLogFile-56.log.gz
MyLogFile-54.log.gz
```


---

_Comment by @BurntSushi on 2021-10-14 11:46_

Please share gz files that provoke this error. Please also show the output of `gzip --help`.

---

_Comment by @golimarrrr on 2021-10-14 12:13_

Don't have the gz files right now but I'll try to get them. The gzip I use is the one from the GnuWin32 project:
```

>gzip --help
Usage: gzip [OPTION]... [FILE]...
Compress or uncompress FILEs (by default, compress FILES in-place).

Mandatory arguments to long options are mandatory for short options too.

  -a, --ascii       ascii text; convert end-of-line using local conventions
  -c, --stdout      write on standard output, keep original files unchanged
  -d, --decompress  decompress
  -f, --force       force overwrite of output file and compress links
  -h, --help        give this help
  -k, --keep        keep (don't delete) input files
  -l, --list        list compressed file contents
  -L, --license     display software license
  -n, --no-name     do not save or restore the original name and time stamp
  -N, --name        save or restore the original name and time stamp
  -q, --quiet       suppress all warnings
  -r, --recursive   operate recursively on directories
  -S, --suffix=SUF  use suffix SUF on compressed files
  -t, --test        test compressed file integrity
  -v, --verbose     verbose mode
  -V, --version     display version number
  -1, --fast        compress faster
  -9, --best        compress better
    --rsyncable   Make rsync-friendly archive

With no FILE, or when FILE is -, read standard input.

Report bugs to <bug-gzip@gnu.org>.

>gzip --version
gzip 1.3.12
Copyright (C) 2007 Free Software Foundation, Inc.
Copyright (C) 1993 Jean-loup Gailly.
This is free software.  You may redistribute copies of it under the terms of
the GNU General Public License <http://www.gnu.org/licenses/gpl.html>.
There is NO WARRANTY, to the extent permitted by law.

Written by Jean-loup Gailly.

```

---

_Comment by @BurntSushi on 2021-10-14 12:22_

Yeah, it's important to provide as much detail as you can in bug reports. We have to rule out this being a problem with your `gzip` installation or the files themselves. ripgrep is just shelling out to `gzip` and passing along the input and output, and the errors if one occurs. So if you're seeing an error from ripgrep, the _very first thing_ you should do is check whether gzip produces that same error: `gzip -d -c < MyLogFile-56.log.gz`.

You also say:

> Combining -z and -l works but there is a seemingly unrelated error in the output

But that's not obviously the case for me. Try searching for something that _doesn't_ exist in those files. Do you get the same output? If so, then nothing is really working at all. You're just seeing errors.

And does this same error occur when _not_ using `-l`?

---

_Comment by @golimarrrr on 2021-11-08 07:25_

I'm attaching a file where it happens; about the questions you asked:
- `gzip -d -c < file.gz` does not produce any error, just outputs the content
- if I search something that doesn't exist there are no errors and also no output (as expected)
- it doesn't occur when not using `-l` , it outputs the matching filenames, line numbers and lines as expected

[aqui.txt.gz](https://github.com/BurntSushi/ripgrep/files/7494946/aqui.txt.gz)


---

_Comment by @BurntSushi on 2023-11-24 20:13_

I can't reproduce the problem:

```
$ tree /tmp/rg2019/
/tmp/rg2019/
└── aqui.txt.gz

1 directory, 1 file

$ rg -zl . /tmp/rg2019/
/tmp/rg2019/aqui.txt.gz
```

---

_Closed by @BurntSushi on 2023-11-24 20:13_

---

_Label `invalid` added by @BurntSushi on 2023-11-24 20:13_

---
