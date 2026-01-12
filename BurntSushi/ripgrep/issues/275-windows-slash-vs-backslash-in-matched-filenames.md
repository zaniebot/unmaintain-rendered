```yaml
number: 275
title: "Windows: slash vs backslash in matched filenames "
type: issue
state: closed
author: ljantzen
labels:
  - question
assignees: []
created_at: 2016-12-08T13:42:19Z
updated_at: 2024-07-29T20:45:26Z
url: https://github.com/BurntSushi/ripgrep/issues/275
synced_at: 2026-01-12T16:13:21Z
```

# Windows: slash vs backslash in matched filenames 

---

_@ljantzen_

I'm on 0.2.3. 

I am using rg in git-bash shell on a win10 box.  It works, but the filenames /directories it outputs follows the windows/dos standard, using the backslash character as directory separator.  This makes it difficult to copy/paste the filename into other commands in the bash shell.  

I could do something like 

     rg -l SearchPattern | sed -e 's/\\/\//g'

but that loses the coloured output and context.

Is there a way to tell rg that I want slash as directory separator character? 



---

_Comment by @BurntSushi on 2016-12-08 13:48_

I doubt it. It shows paths as they are supposed to be shown for your particular platform. Why doesn't your Bash shell handle Windows paths?

---

_Label `question` added by @BurntSushi on 2016-12-08 13:48_

---

_Comment by @BurntSushi on 2016-12-12 12:00_

It's not clear to me what specific problem the OP is facing here and whether ripgrep is the one to blame. If there's more clarity that can be provided, please feel free to re-open!

---

_Closed by @BurntSushi on 2016-12-12 12:00_

---

_Comment by @ljantzen on 2016-12-12 14:53_

I agree that running bash on windows perhaps isn't the most common use-case, but it is pretty common at least around where I am ( .net appl dev using git ).   I have installed msysgit, which is the most common git command line client on windows.  It provides a pretty standard bash shell, where I can access the usual bash/unixy commands and run shell scripts.  As a former unix/linux based developer converting to .net I find that is a tremendous productivity benefit. I live in bash, even when I'm on windows. 

I was really excited when I found ripgrep.  As a unix junkie, I find it really hits the sweet spot.  It even works on windows on the command line. Yeah!  Bash? No problem, it rips through the source code in no time. When ripgrep finds something interesting, I usually want to open the file to edit or take a closer look.  But, bash uses forward slashes, whereas dos/windows uses backslash as the directory separator character.  In bash the backslash is the escape character, and requires special handling.  '\t' is the tab character.  So now I need to copy and paste the direcory/filename and manually replace the backslashes with slashes before I can open the file in vim or whatever. 

Cutting and pasting a standard windows directory name into a bash shell simply does not work as you would hope.  Is that a missing feature of bash? I could argue that with the bash/unix maintainers, but I doubt I would make much headway in that discussion. 

I would really appreciate a '-XUseUnixDirsep' option or something similar to force ripgrep to ouput src/test/apptest.cs instead of src\test\apptest.cs.   Would you accept a pull request to that effect? 


 

    

---

_Comment by @BurntSushi on 2016-12-12 17:04_

Is ripgrep really the only tool with this problem? How do you deal with other tools that output paths using the separator defined by your platform?

> I would really appreciate a '-XUseUnixDirsep' option or something similar to force ripgrep to ouput src/test/apptest.cs instead of src\test\apptest.cs. Would you accept a pull request to that effect?

Possibly... But it would have to be easy to maintain and not (measurably) impact the performance of ripgrep when used without the option enabled. (A branch in the code is fine, but an unconditional search-and-replace-and-allocate is not.) My guess is that the easiest place to implement this is in the printer. And if you're going to do it, you might as well permit the user to specify the path separator.

---

_Reopened by @BurntSushi on 2016-12-12 17:04_

---

_Comment by @cormacrelf on 2016-12-23 00:21_

> Is ripgrep really the only tool with this problem? How do you deal with other tools that output paths using the separator defined by your platform?

I use git-bash when I'm on Windows 10. I almost never use any tools that output or even accept backslashes, I just have most of the UNIX standard tools and use them as a total replacement for cmd. Ripgrep is the only tool I use day-to-day which doesn't prefer UNIX paths, and currently I just revert to `grep -rl` when I need to use paths. I would appreciate a flag that I can `alias rg="rg --pathsep '/'"` or similar.

---

_Closed by @BurntSushi on 2017-01-10 23:22_

---

_Comment by @cormacrelf on 2017-01-11 10:54_

Thanks! Much appreciated.

---

_Comment by @ljantzen on 2017-03-21 14:15_

Yay!  Thank you 👍 

---

_Comment by @ljantzen on 2017-03-24 12:03_

As a follow-up note, I found this to be working, although in a slightly surprising fashion. 

```/c/dev/tmp/rg$ rg -i xxx
a.txt
1:XXX=1

subdir\b.txt
1:XXX=2
/c/dev/tmp/rg$ rg --path-separator % -i xxx
a.txt
1:XXX=1

subdir%b.txt
1:XXX=2
/c/dev/tmp/rg$ rg --path-separator \\ -i xxx
a.txt
1:XXX=1

subdir\b.txt
1:XXX=2
/c/dev/tmp/rg$ rg --path-separator ! -i xxx
a.txt
1:XXX=1

subdir!b.txt
1:XXX=2
/c/dev/tmp/rg$ rg --path-separator / -i xxx
A path separator must be exactly one byte, but the given separator is 16 bytes.
/c/dev/tmp/rg$ rg --path-separator \/ -i xxx
A path separator must be exactly one byte, but the given separator is 16 bytes.
/c/dev/tmp/rg$ rg --path-separator // -i xxx
a.txt
1:XXX=1

subdir/b.txt
1:XXX=2
```

---

_Comment by @BurntSushi on 2017-03-24 12:09_

@ljantzen My guess is that it's unrelated to ripgrep. Are you in cygwin? Are you sure cygwin isn't trying to do some fancy path translation on `/` that's causing it to expand to something else?

---

_Comment by @ljantzen on 2017-03-24 12:14_

I agree, it probably is unrelated to ripgrep.  I am using the bash shell that comes installed with git, 'Git Bash'.  I just wanted to document this for others who might stumble on this as well. Thanks for an awesome tool 😄   

---

_Comment by @BatmanAoD on 2017-10-02 18:18_

Yeah, this definitely looks like a piece of Git-Bash "magic". Adding the `sep` argument itself to the error message, I get:

    $> rg.exe --path-separator / 'pattern'
    A path separator must be exactly one byte, but the given separator `C:/Program Files/Git/` is 21 bytes.

...which is...just awesome. :roll_eyes: 

Anyway, `//` works great. Thanks!

---

_Comment by @cha5on on 2024-07-29 20:45_

In case anyone else using bazel + git bash (msys) happens to stumble across this issue and is wondering why it doesn't work...

```
$ rg --path-separator // 'pattern'
A path separator must be exactly one byte, but the given separator is 2 bytes: //
In some shells on Windows '/' is automatically expanded. Use '//' instead.
```

...it's likely that you put `export MSYS2_ARG_CONV_EXCL="//"` or similar in your bashrc to avoid it getting turned into `/` when working with bazel. Here's an alias that unsets that and will work:

```
alias rg='MSYS2_ARG_CONV_EXCL="" rg --path-separator //'
```

---
