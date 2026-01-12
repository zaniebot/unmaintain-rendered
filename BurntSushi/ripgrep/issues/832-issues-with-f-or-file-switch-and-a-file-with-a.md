```yaml
number: 832
title: Issues with -f or --file switch and a file with a backslash character (\)
type: issue
state: closed
author: leftcoastbeard
labels:
  - question
  - doc
assignees: []
created_at: 2018-02-23T01:25:52Z
updated_at: 2018-02-23T17:18:36Z
url: https://github.com/BurntSushi/ripgrep/issues/832
synced_at: 2026-01-12T16:13:22Z
```

# Issues with -f or --file switch and a file with a backslash character (\)

---

_@leftcoastbeard_

#### What version of ripgrep are you using?

ripgrep 0.8.1
-SIMD -AVX

#### What operating system are you using ripgrep on?

Windows 10 x64, 1709

#### Describe your question, feature request, or bug.

```
-f or --file switch causing issues with files that have back slash character (\)
```
#### If this is a bug, what are the steps to reproduce the behavior?


Using file.txt as this:
```
This is text \fixme \hello \break \choose, etc...
```
This produces:
```
rg hello -f file.txt
Error parsing regex near 'xme \hello' at character offset 21: Unrecognized escape sequence: '\h'.
(Hint: Try the --fixed-strings flag to search for a literal string.)
```
If I run without the -f switch I get:
```
rg hello file.txt
1:This is text \fixme \hello \break \choose, etc...
```

Another weird error: test.txt 
```
This is a test string.
With slashes\forward in it.
Going to break\rg.
```
Running:
```
rg slashes -f test.txt
slashes: IO error for operation on slashes: The system cannot find the file specified. (os error 2)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

Should have done this:
```
rg slashes test.txt
2:With slashes\forward in it.
```
#### If this is a bug, what is the actual behavior?

Debug for first:
```
rg text -f file.txt --debug
Error parsing regex near 'xme \hello' at character offset 25: Unrecognized escape sequence: '\h'.
(Hint: Try the --fixed-strings flag to search for a literal string.)
```
Debug for second:
[Gist Here](https://gist.github.com/leftcoastbeard/d2d480972c3d88f34107f7e6571b2cf6)

The --debug stopped working after this dumped (couldn't replicate the debug output again).
Show the command you ran and the actual output. Include the `--debug` flag in
your invocation of ripgrep.


---

_Comment by @leftcoastbeard on 2018-02-23 01:31_

Am I misinterpreting the usage of the -f/--file parameter? Am I supposed to be using the file as my search query? **facepalm**

EDIT: Yup. The Slashes example is still an issue though.

---

_Comment by @okdana on 2018-02-23 02:23_

The `-f` file contains a list of patterns that `rg` should search for (instead of using a pattern supplied directly on the command line). For example, these are basically equivalent searches:

```
# Pattern supplied on command line
% rg 'foo.*bar' file1 file2 file3

# Pattern supplied via file with `-f`
% cat patterns
foo.*bar
% rg -f patterns file1 file2 file3
```

The patterns in the file have to follow the same regex syntax rules as a pattern supplied on the command line. In Rust's regex engine, the `\` character can be used either to make characters that are normally special *not* special (for example, `\*` matches a literal `*` character) or the other way around (for example, `\d` matches a digit). Since `\` itself is therefore a special character, you need to escape it to make it match a literal `\` in your file.

So, instead of `\fixme` (or whatever), make your pattern `\\fixme`.

Alternatively, you might be able to use the `-F` option to make `rg` treat patterns as literal strings (see `rg --help`).

---

_Comment by @leftcoastbeard on 2018-02-23 03:36_

I appreciate the feedback, this clears up my misunderstanding. I think you can probably close this issue as I was not using the switch correctly.

- Paul Hey
From Outlook<https://aka.ms/qtex0l> iOS
________________________________
From: dana <notifications@github.com>
Sent: Thursday, February 22, 2018 6:23:05 PM
To: BurntSushi/ripgrep
Cc: Paul Hey; Author
Subject: Re: [BurntSushi/ripgrep] Issues with -f or --file switch and a file with a backslash character (\) (#832)


The -f file contains a list of patterns that rg should search for (instead of using a pattern supplied directly on the command line). For example, these are basically equivalent searches:

# Pattern supplied on command line
% rg 'foo.*bar' file1 file2 file3

# Pattern supplied via file with `-f`
% cat patterns
foo.*bar
% rg -f patterns file1 file2 file3


The patterns in the file have to follow the same regex syntax rules as a pattern supplied on the command line. In Rust's regex engine, the \ character can be used either to make characters that are normally special not special (for example, \* matches a literal * character) or the other way around (for example, \d matches a digit). Since \ itself is therefore a special character, you need to escape it to make it match a literal \ in your file.

So, instead of \fixme (or whatever), make your pattern \\fixme.

Alternatively, you might be able to use the -F option to make rg treat patterns as literal strings (see rg --help).

—
You are receiving this because you authored the thread.
Reply to this email directly, view it on GitHub<https://github.com/BurntSushi/ripgrep/issues/832#issuecomment-367889855>, or mute the thread<https://github.com/notifications/unsubscribe-auth/AOfyKqfsXiL21-gDOINxZpDMQ-BxiVHiks5tXiEIgaJpZM4SQQGu>.


---

_Comment by @BurntSushi on 2018-02-23 11:50_

@leftcoastbeard Thanks for the bug report! Could you please help us understand how we can improve ripgrep's documentation such that it would have prevented this confusion?

---

_Label `question` added by @BurntSushi on 2018-02-23 11:50_

---

_Label `doc` added by @BurntSushi on 2018-02-23 11:50_

---

_Comment by @leftcoastbeard on 2018-02-23 17:11_

I think the simplest way would be to update the Usage options from:
```
USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f FILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
```
To something like:
```
USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
```
The full help has it explained clearly, more or less. I just needed to RTM.

Thanks,

-Paul Hey

From: Andrew Gallant [mailto:notifications@github.com]
Sent: February 23, 2018 03:50
To: BurntSushi/ripgrep <ripgrep@noreply.github.com>
Cc: Paul Hey <HeyP@camosun.bc.ca>; Mention <mention@noreply.github.com>
Subject: Re: [BurntSushi/ripgrep] Issues with -f or --file switch and a file with a backslash character (\) (#832)


@leftcoastbeard<https://github.com/leftcoastbeard> Thanks for the bug report! Could you please help us understand how we can improve ripgrep's documentation such that it would have prevented this confusion?

—
You are receiving this because you were mentioned.
Reply to this email directly, view it on GitHub<https://github.com/BurntSushi/ripgrep/issues/832#issuecomment-367988932>, or mute the thread<https://github.com/notifications/unsubscribe-auth/AOfyKhi0lXu-sl2tZuOKF8mvfWHpigZVks5tXqX3gaJpZM4SQQGu>.


---

_Closed by @BurntSushi on 2018-02-23 17:18_

---

_Comment by @BurntSushi on 2018-02-23 17:18_

@leftcoastbeard Done! Thanks.

---
