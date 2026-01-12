```yaml
number: 318
title: Many file skipped for unknown reasons
type: issue
state: closed
author: lzybkr
labels:
  - bug
assignees: []
created_at: 2017-01-13T21:15:11Z
updated_at: 2017-01-18T03:21:05Z
url: https://github.com/BurntSushi/ripgrep/issues/318
synced_at: 2026-01-12T16:13:21Z
```

# Many file skipped for unknown reasons

---

_@lzybkr_

On Windows 10, I'm seeing fewer results than expected, plus fewer matches when ignoring case.

```
C:\Windows>cd %SYSTEMROOT%\System32\WindowsPowerShell\v1.0\Modules

C:\Windows\System32\WindowsPowerShell\v1.0\Modules>rg Copyright
AssignedAccess\AssignedAccess.psd1
7:Copyright = '(c) Microsoft Corporation. All rights reserved.'

AppBackgroundTask\AppBackgroundTask.psd1
5:    Copyright = '
C:\Windows\System32\WindowsPowerShell\v1.0\Modules>rg -i Copyright
AppBackgroundTask\AppBackgroundTask.psd1
5:    Copyright = '
C:\Windows\System32\WindowsPowerShell\v1.0\Modules>findstr /S Copyright *.psd1
AppBackgroundTask\AppBackgroundTask.psd1:    Copyright = '⌐ Microsoft Corporation. All rights reserved.'
AppvClient\AppvClient.psd1:Copyright = '⌐ Microsoft Corporation. All rights reserved.'
Appx\Appx.psd1:    Copyright = "┬⌐ Microsoft Corporation. All rights reserved."
AssignedAccess\AssignedAccess.psd1:Copyright = '(c) Microsoft Corporation. All rights reserved.'
BranchCache\BranchCache.psd1:    Copyright="⌐ Microsoft Corporation. All rights reserved."
ConfigCI\ConfigCI.psd1:Copyright="⌐ Microsoft Corporation. All rights reserved."
Containers\1.0.0.0\Containers.psd1:# Copyright statement for this module
Containers\1.0.0.0\Containers.psd1:Copyright = '(c) 2015 Microsoft Corporation. All rights reserved.'
Defender\Defender.psd1:    Copyright="⌐ Microsoft Corporation. All rights reserved."
...
```

Notice 2 matches from the case sensitive search, 1 from the case-insensitive search, and many matches using findstr (with many omitted.)

---

_Comment by @BurntSushi on 2017-01-13 21:43_

Can you run `rg` with the `--debug` flag and show the output here?

Also, what version of ripgrep are you running? See `rg --version`.

---

_Comment by @lzybkr on 2017-01-13 21:46_

Sure:

```
#5 PS> rg --debug Copyright
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'C',
        'o',
        'p',
        'y',
        'r',
        'i',
        'g',
        'h',
        't'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(Copyright)], limit_size: 250, limit_class: 10 }
AssignedAccess\AssignedAccess.psd1
7:Copyright = '(c) Microsoft Corporation. All rights reserved.'

AppBackgroundTask\AppBackgroundTask.psd1
5:    Copyright = '
@ C:\Windows\System32\WindowsPowerShell\v1.0\Modules
#6 PS> rg --debug -i Copyright
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'C',
        'o',
        'p',
        'y',
        'r',
        'i',
        'g',
        'h',
        't'
    ],
    casei: true
}
DEBUG:grep::literals: required literals found: [Cut(COPYR), Cut(cOPYR), Cut(CoPYR), Cut(coPYR), Cut(COpYR), Cut(cOpYR), Cut(CopYR), Cut(copYR), Cut(COPyR), Cut(cOPyR), Cut(CoPyR), Cut(coPyR), Cut(COpyR), Cut(cOpyR), Cut(CopyR), Cut(copyR), Cut(COPYr), Cut(cOPYr), Cut(CoPYr), Cut(coPYr), Cut(COpYr), Cut(cOpYr), Cut(CopYr), Cut(copYr), Cut(COPyr), Cut(cOPyr), Cut(CoPyr), Cut(coPyr), Cut(COpyr), Cut(cOpyr), Cut(Copyr), Cut(copyr)]
AssignedAccess\AssignedAccess.psd1
7:Copyright = '(c) Microsoft Corporation. All rights reserved.'

AppBackgroundTask\AppBackgroundTask.psd1
5:    Copyright = '
@ C:\Windows\System32\WindowsPowerShell\v1.0\Modules
#7 PS> rg --version
ripgrep 0.3.2
```

This is the latest release (I think) - I also tested with something I built after from my PR which I think is newer than this release, but saw the same results.

---

_Comment by @BurntSushi on 2017-01-13 22:48_

Interestingly, `rg -i Copyright` in your second comment has different output than `rg -i Copyright` in your first comment. Are the results changing from run to run?

Is there anyway you can reproduce this on a public repo of code so I can try?

---

_Comment by @lzybkr on 2017-01-13 23:52_

Try exactly what I posted in my first message - you should see similar results on most Windows machines - I used the latest supported Windows 10 release, but I would expect similar results in earlier versions of Windows.

---

_Comment by @BurntSushi on 2017-01-14 00:07_

Hmm, your directory has a lot more stuff in it than my Windows 7 VM. Let me boot up my Windows 10 laptop.

---

_Comment by @BurntSushi on 2017-01-14 00:28_

Did you compile ripgrep from source? Or did you download a [release](https://github.com/BurntSushi/ripgrep/releases)?

---

_Comment by @BurntSushi on 2017-01-14 01:52_

I think I've figured out the problem here. If you do this: `rg Copyright > output` and then look at the contents of `output`, do you see the results you expect?

If so, then I think the problem here is that some of the files you're searching contain invalid UTF-8, and Rust's standard library by default returns an error if you try to write invalid UTF-8 to the console. (The reason why is because UTF-8 needs to be transcoded to UTF-16 to write to the console.) I think the right answer here is to lossily decode the UTF-8 before sending it to `stdout`.

This UTF-8 check doesn't happen if you write to a file, which is why redirecting to stdout should produce the expected output.

The reason why no error is printed is because ripgrep isn't checking an error when writing to stdout, which is bug #200.

---

_Comment by @BurntSushi on 2017-01-14 02:21_

In PowerShell, if I do `rg foo | echo`, then I get the correct output.

---

_Label `bug` added by @BurntSushi on 2017-01-14 04:00_

---

_Closed by @BurntSushi on 2017-01-14 04:21_

---

_Comment by @lzybkr on 2017-01-18 03:21_

Much better now, thanks.

---
