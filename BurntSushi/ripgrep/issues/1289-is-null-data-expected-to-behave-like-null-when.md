```yaml
number: 1289
title: Is --null-data expected to behave like --null when printing file path?
type: issue
state: open
author: learnbyexample
labels:
  - question
assignees: []
created_at: 2019-05-30T06:23:45Z
updated_at: 2020-01-22T10:15:09Z
url: https://github.com/BurntSushi/ripgrep/issues/1289
synced_at: 2026-01-12T16:13:23Z
```

# Is --null-data expected to behave like --null when printing file path?

---

_@learnbyexample_

#### What version of ripgrep are you using?

```bash
$ rg --version
ripgrep 11.0.1 (rev 1f1cd9b467)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)
```

#### How did you install ripgrep?

Using https://github.com/BurntSushi/ripgrep/releases/download/11.0.1/ripgrep_11.0.1_amd64.deb

#### What operating system are you using ripgrep on?

Ubuntu 16.04.1

#### Describe your question, feature request, or bug.

When `--null-data` option is used (but without using `--null`) and filenames are part of output, such file paths are terminated with NUL byte, similar to what `--null` does. Is this expected behavior?

```bash
$ echo 'how are you?' > f1.txt
$ echo 'who are you?' > f2.txt

$ rg --null-data -l 'are' f1.txt f2.txt
f1.txtf2.txt$ 

$ rg --null-data 'are' f1.txt f2.txt
f1.txt1:how are you?

f2.txt1:who are you?

$ rg --null-data -l 'are' f1.txt f2.txt | od -c
0000000   f   1   .   t   x   t  \0   f   2   .   t   x   t  \0
0000016
$ rg --null-data --heading 'are' f1.txt f2.txt | od -c
0000000   f   1   .   t   x   t  \0   h   o   w       a   r   e       y
0000020   o   u   ?  \n  \0  \n   f   2   .   t   x   t  \0   w   h   o
0000040       a   r   e       y   o   u   ?  \n  \0
0000053
```

---

_Comment by @BurntSushi on 2019-05-30 11:03_

Interesting! Thanks for filing this.

The problem here is that when ripgrep prints file headings (instead of the full file path for each matching line), it needs to print a new line after the file path. Currently, the new line character used for that is the newline character set by the end user. In this case, the `--null-data` flag explicitly and intentionally sets the newline character to `\x00`. So that when printing the heading, the newline character used is `\x00` instead of `\n`. The same logic applies when printing file paths, as the newline character that was set is used there as well.

I wonder what the correct behavior should be. The current behavior is indeed surprising, but consistent. Is the correct behavior to just assume `\n` as a line terminator when printing headings and file paths, unless `--null` is given?

---

_Label `question` added by @BurntSushi on 2019-05-30 11:03_

---

_Comment by @learnbyexample on 2019-05-30 11:50_

Personally, I don't have a use case, so I don't have a preference and this could just be added to manual as a note. Couple of points to consider:

* Filename and line number combination like `f1.txt1:` looks odd because NUL isn't visible on terminal
* User can always add `--null` option to get NUL separated paths, whereas if they want newline separated ones, there isn't a way to do it (as far as I know)

For reference, as far as I've noticed, GNU grep uses `-z` to change line separator only for input data processing and adding the NUL after each output data match. File names are separated by newline as usual.


---

_Comment by @johnnytemp on 2020-01-22 06:52_

I also experienced the similar issue - with `--null-data`, but not using `-l`. In my case, I want to have normal newline (`\n`) to separate results in the output, unless --null is explicitly given. Otherwise, the output will be garbage - not displayed correctly line-by-line. Even pipeline to `wc -l` doesn't work for that output. (Current temporary fix: can only pipeline to `tr '\0' '\n'`, which breaks terminal coloring and some behaviors)

Changing the input data as null-terminated not just mean an I/O stream of lines terminated by null. It can also mean we want a textual file not to be separated into lines, so that we could use `rg` to match only first line, e.g. search the linux shebang pattern by `rg --null-data -o '\A#!/.*'`. It doesn't always imply user want the output to be garbage with `\0` separators.

---

_Comment by @johnnytemp on 2020-01-22 07:15_

### About the output problem I mentioned above

I just double checked. Output without pipeline is still line-by-line, it is NOT ALWAYS garbage (or hard-to-read, I should say). I found that in the output of `rg --null-data -o '\A#!/.*'` (without further pipeline!), the first-character of each line (the filename part) disappeared due to a `\r<some-color-escapes>\0` sequence (just before '\n') (my files is in DOS line ending). (Note: '\r' make cursor return home, and '\0' print a blank). _Should a "." better don't match both \r & \n by default? (This doesn't harm linux environment)_

i.e. expected correct output:

    file1.sh 1:#!/bin/bash
    file2.sh 1:#!/bin/bash
    file3.sh 1:#!/bin/bash

become:

     ile1.sh 1:#!/bin/bash
     ile2.sh 1:#!/bin/bash
     ile2.sh 1:#!/bin/bash

In addition, I've checked that with pipeline or not, the behavior is different. With a further `| cat`, the output of `rg` is always single-lined (no `\n` but only '\r' & '\0'). These two behaviors seems confusing...

`rg --version` -> 11.0.2

---

_Comment by @johnnytemp on 2020-01-22 07:46_

Info: for `grep`, '--null-data' or '-z' -> `Treat  input  and  output  data  as  sequences  of lines, each terminated by a zero byte`. Thus, I cannot say `rg` is wrong, because current behavior align with `grep`. Just not quite convenient for the mentioned use cases.

---
