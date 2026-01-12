```yaml
number: 1837
title: rg skips one of the files when using glob
type: issue
state: closed
author: igorsol
labels: []
assignees: []
created_at: 2021-03-30T13:04:33Z
updated_at: 2021-03-30T14:28:54Z
url: https://github.com/BurntSushi/ripgrep/issues/1837
synced_at: 2026-01-12T16:13:24Z
```

# rg skips one of the files when using glob

---

_@igorsol_

#### What version of ripgrep are you using?

    ripgrep 12.1.1 (rev 7cb211378a)
    -SIMD -AVX (compiled)
    +SIMD +AVX (runtime)

#### How did you install ripgrep?

    curl -LO https://github.com/BurntSushi/ripgrep/releases/download/12.1.1/ripgrep_12.1.1_amd64.deb
    sudo dpkg -i ripgrep_12.1.1_amd64.deb

#### What operating system are you using ripgrep on?

Ubuntu 16.04

#### Describe your bug.

I have several log files. I want to count number of occurrences of word 'recovery' in each file. It seems rg completely ignores the first file matching the glob pattern.

#### What are the steps to reproduce the behavior?

For example content of the start1.log is like this (but I think this bug is not related to files content):
```
....
recovery
,,,,,
word recovery end
--- recovery
```

If I provide file name without special glob characters then word 'recovery' is found in each file:

    $ rg recovery -u -i -g restore-test/shard01/start1.log --count-matches
    restore-test/shard01/start1.log:3
    $ rg recovery -u -i -g restore-test/shard01/start2.log --count-matches
    restore-test/shard01/start2.log:1373
    $ rg recovery -u -i -g restore-test/shard01/start3.log --count-matches
    restore-test/shard01/start3.log:126
    $ rg recovery -u -i -g restore-test/shard01/start4.log --count-matches
    restore-test/shard01/start4.log:20

But if I try to use '?' glob character then rg does not print anything about the 'start1.log':

    $ rg recovery -u -i -g restore-test/shard01/start?.log --count-matches
    restore-test/shard01/start4.log:20
    restore-test/shard01/start3.log:126
    restore-test/shard01/start2.log:1373

#### What is the actual behavior?

With `--debug` parameter output is like this:
```
$ rg recovery -u -i -g restore-test/shard01/start?.log --count-matches --debug
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:109: required literals found: [Cut(RECOV), Cut(rECOV), Cut(ReCOV), Cut(reCOV), Cut(REcOV), Cut(rEcOV), Cut(RecOV), Cut(recOV), Cut(RECoV), Cut(rECoV), Cut(ReCoV), Cut(reCoV), Cut(REcoV), Cut(rEcoV), Cut(RecoV), Cut(recoV), Cut(RECOv), Cut(rECOv), Cut(ReCOv), Cut(reCOv), Cut(REcOv), Cut(rEcOv), Cut(RecOv), Cut(recOv), Cut(RECov), Cut(rECov), Cut(ReCov), Cut(reCov), Cut(REcov), Cut(rEcov), Cut(Recov), Cut(recov)]
DEBUG|grep_regex::matcher|crates/regex/src/matcher.rs:50: extracted fast line regex: (?-u:RECOV|rECOV|ReCOV|reCOV|REcOV|rEcOV|RecOV|recOV|RECoV|rECoV|ReCoV|reCoV|REcoV|rEcoV|RecoV|recoV|RECOv|rECOv|ReCOv|reCOv|REcOv|rEcOv|RecOv|recOv|RECov|rECov|ReCov|reCov|REcov|rEcov|Recov|recov)
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|restore-test/shard01/start4.log:20
globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
restore-test/shard01/start3.log:126
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
restore-test/shard01/start2.log:1373
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```
The `echo` command prints all file names:
```
$ echo restore-test/shard01/start?.log
restore-test/shard01/start1.log restore-test/shard01/start2.log restore-test/shard01/start3.log restore-test/shard01/start4.log
```

#### What is the expected behavior?

I think ripgrep's output should be like this:

    $ rg recovery -u -i -g restore-test/shard01/start?.log --count-matches
    restore-test/shard01/start4.log:20
    restore-test/shard01/start3.log:126
    restore-test/shard01/start2.log:1373
    restore-test/shard01/start1.log:3


---

_Comment by @BurntSushi on 2021-03-30 13:10_

Is that the full debug output?

Do you have a ripgrep config file in use?

Otherwise, please see about providing a minimal reproduction that others can try.

---

_Comment by @BurntSushi on 2021-03-30 13:15_

Also, why isn't your glob in quotes? Without quoting it, it will be interpreted by your shell.

---

_Comment by @igorsol on 2021-03-30 14:02_

> Is that the full debug output?

Yes, that is the full debug output

> Do you have a ripgrep config file in use?

No

> Also, why isn't your glob in quotes? Without quoting it, it will be interpreted by your shell.

That was my fault. With quoted glob everything works as expected.
Thank you.
Closing the issue.

---

_Closed by @igorsol on 2021-03-30 14:02_

---
