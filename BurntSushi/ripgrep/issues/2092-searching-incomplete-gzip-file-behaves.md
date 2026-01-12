```yaml
number: 2092
title: Searching incomplete gzip file behaves differently between one file and multiple files
type: issue
state: open
author: willlllllio
labels: []
assignees: []
created_at: 2021-11-30T03:17:52Z
updated_at: 2021-12-04T03:50:40Z
url: https://github.com/BurntSushi/ripgrep/issues/2092
synced_at: 2026-01-12T16:13:24Z
```

# Searching incomplete gzip file behaves differently between one file and multiple files

---

_@willlllllio_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)


#### How did you install ripgrep?

cargo install ripgrep

#### What operating system are you using ripgrep on?

1) Ubuntu 20.04.2 LTS
2) MacOs 11.5.1
Tested on both, same behaviour on both minus the one extra error line.
#### Describe your bug.

Incomplelte gzip files (.gz that are valid for a while but cut off early) only get searched when given as single file but not when searching multiple files or a directory.

#### What are the steps to reproduce the behavior?

Creating an example gzip file that cuts off about halfway through, nothing else in the same dir:
`seq 10000000 |gzip -5 |head -c 2123123 >1.gz`

Searching the file directly:

``` 
$ rg -zuuuu 12345 1.gz
12345:12345
112345:112345
123450:123450
123451:123451
123452:123452
123453:123453
123454:123454
123455:123455
123456:123456
123457:123457
123458:123458
123459:123459
212345:212345
312345:312345
412345:412345
512345:512345
612345:612345
712345:712345
812345:812345
912345:912345
1.gz:
-------------------------------------------------------------------------------
gzip: 1.gz: unexpected end of file
-------------------------------------------------------------------------------
```

Searching the dir (linux):
```
$ rg -zuuuu 12345 ./
./1.gz:
-------------------------------------------------------------------------------
gzip: ./1.gz: unexpected end of file
-------------------------------------------------------------------------------
```

Searching the whole dir (Mac, same behaviour but extra error line):
```
$ rg -zuuuu 12345 .
./1.gz:
-------------------------------------------------------------------------------
gzip: ./1.gz: unexpected end of file
gzip: ./1.gz: uncompress failed
-------------------------------------------------------------------------------
``` 

trying with a 2nd file:
```
cp 1.gz 2.gz
$ rg -zuuuu 12345 ./
./1.gz:
-------------------------------------------------------------------------------
gzip: ./1.gz: unexpected end of file
gzip: ./1.gz: uncompress failed
-------------------------------------------------------------------------------
./2.gz:
-------------------------------------------------------------------------------
gzip: ./2.gz: unexpected end of file
gzip: ./2.gz: uncompress failed
-------------------------------------------------------------------------------

$ rg -zuuuu 12345 1.gz 2.gz
2.gz:
-------------------------------------------------------------------------------
gzip: 2.gz: unexpected end of file
gzip: 2.gz: uncompress failed
-------------------------------------------------------------------------------
1.gz:
-------------------------------------------------------------------------------
gzip: 1.gz: unexpected end of file
gzip: 1.gz: uncompress failed
-------------------------------------------------------------------------------
```

#### What is the actual behavior?

Searching `1.gz` specifically works correctly and reads as far as seemingly possible, but when searching the dir or searching two specific files doesn't find anything.

#### What is the expected behavior?

Get equally as far in the cut off gzip file when searching the full dir or multiple files as when searching one file specifically.

I get that searching broken gzip files that cut off early is kind of a weirdish thing in the first place (but really useful when something is writing .gz log files directly or you're searching files while copying/syncing them still), but it seems weird that it works when it's just the single file given as argument but not when searching multiple.




---

_Comment by @willlllllio on 2021-12-04 03:50_

After more testing seems the exit code is what causes the change?
When wrapping gzcat in a script that exits with 0 it works as expected, just as a quick workaround in case anyone else ends up here looking for the same issue.

```bash
#!/bin/bash

gzcat "$@"
exit 0
```

`rg -zuuuu 12345 . --pre ./gzcat.sh`

```
./2.gz
12345:12345
112345:112345
123450:123450
...
912345:912345

./1.gz
12345:12345
112345:112345
...
912345:912345
```


---
