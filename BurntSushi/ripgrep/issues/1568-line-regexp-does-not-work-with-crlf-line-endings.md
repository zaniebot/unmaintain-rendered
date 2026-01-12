```yaml
number: 1568
title: "--line-regexp does not work with CRLF line endings"
type: issue
state: closed
author: jftuga
labels:
  - duplicate
assignees: []
created_at: 2020-05-05T22:16:22Z
updated_at: 2020-05-05T23:00:36Z
url: https://github.com/BurntSushi/ripgrep/issues/1568
synced_at: 2026-01-12T16:13:23Z
```

# --line-regexp does not work with CRLF line endings

---

_@jftuga_

#### What version of ripgrep are you using?

ripgrep 12.0.1 (rev 1d5b1011e5)
-SIMD -AVX (compiled)

#### How did you install ripgrep?
From the Github binary releases.

#### What operating system are you using ripgrep on?

Windows 10 - 10.0.17763.1158

#### Describe your bug.

Using a text file with CRLF line endings, which is typical for `cmd.exe`, the `--line-regexp` will not match an entire line.

#### What are the steps to reproduce the behavior?

```
R:\>type con > test
one
two
^Z

:: Notice that `test` contains CRLF
R:\>od -tx1 test
0000000 6f 6e 65 0d 0a 74 77 6f 0d 0a
0000012

:: this should return line 1, but does not
R:\>rg -x one test

R:\>copy test test2
        1 file(s) copied.

:: zip will convert CR LF to just LF
R:\>zip -ll z.zip test2 && unzip -o z.zip
  adding: test2 (172 bytes security) (stored 0%)
Archive:  z.zip
 extracting: test2

:: Notice `test2` now only has LF
R:\>od -tx1 test2
0000000 6f 6e 65 0a 74 77 6f 0a
0000010

:: Notice 'rg' now works as expected
R:\>rg -x one test2
1:one
```

#### What do you think ripgrep should have done?

`rg` should be able to handle `CRLF` as a proper line ending, but it does not.



---

_Comment by @okdana on 2020-05-05 22:41_

```
% printf '%s\r\n' foo bar | rg -x foo
% printf '%s\r\n' foo bar | rg --crlf -x foo
foo
```

```
--crlf
    When enabled, ripgrep will treat CRLF (\r\n) as a line terminator
    instead of just \n.

    Principally, this permits $ in regex patterns to match just before
    CRLF instead of just before LF. The underlying regex engine may not
    support this natively, so ripgrep will translate all instances of $
    to (?:\r??$). This may produce slightly different than desired
    match offsets. It is intended as a work-around until the regex
    engine supports this natively.

    CRLF support can be disabled with --no-crlf.
```

---

_Comment by @jftuga on 2020-05-05 22:56_

Shouldn't this be the default for `Windows` builds?  As a Windows user, having to add `--crlf`` all of the time does not seem very user friendly.  It's also likely to be forgotten that it is required.

I just tested and it does work for me.

---

_Comment by @BurntSushi on 2020-05-05 23:00_

You can add it to your ripgrep config file.

It isn't enabled by default because it has weird semantics in corner cases due to how its implemented. The docs point this out.

---

_Closed by @BurntSushi on 2020-05-05 23:00_

---

_Label `duplicate` added by @BurntSushi on 2020-05-05 23:00_

---
