```yaml
number: 2438
title: bug in passthru option
type: issue
state: closed
author: elkarouh
labels:
  - duplicate
assignees: []
created_at: 2023-03-01T15:22:13Z
updated_at: 2023-03-02T13:37:53Z
url: https://github.com/BurntSushi/ripgrep/issues/2438
synced_at: 2026-01-12T16:13:24Z
```

# bug in passthru option

---

_@elkarouh_

#### What version of ripgrep are you using?
ripgrep 13.0.0 (rev af6b6c543b)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
#### How did you install ripgrep?
manually
#### What operating system are you using ripgrep on?
3.10.0-1160.31.1.el7.x86_64 #1 SMP Wed May 26 20:18:08 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
#### Describe your bug.
Non-matching lines duplicated in the output, when using multiline options
#### What are the steps to reproduce the behavior?
I am trying to solve the problem of moving a line as described in the following url
https://stackoverflow.com/questions/10499960/how-to-move-specified-line-in-file-to-other-place-with-regexp-match-bash-script
I then run the following command:
The EXAMPLE file contains :
asdasd0
asdasd1
asdasd2
asdasd3
asdasd4
asdasd5
asdasd6
```
rg --debug --multiline --multiline-dotall  '^(asdasd1.*)(asdasd5(?:\n|$))'  --passthru -r '$2$1'  EXAMPLE
Output is:
<multiline-dotall  '^(asdasd1.*)(asdasd5(?:\n|$))' inepassthru -r '$2$1'  EXAMPLE
DEBUG|rg::config|crates/core/config.rs:40: /auto/home/elkarouh/.config/ripgrep/rc: arguments loaded from config file: [ "--type-add", "templ:*.templ"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--type-add", "templ:*.templ", "--debug", "--multiline"
, "--multiline-dotall", "^(asdasd1.*)(asdasd5(?:\\n|$))", "--passthru", "-r", "$2$1", "EXAMPLE"]
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions,
 0 regexes
asdasd0
asdasd5
asdasd1
asdasd2
asdasd3
asdasd4
asdasd6

asdasd6

```

#### What is the expected behavior?
Expected output is:
asdasd0
asdasd5
asdasd1
asdasd2
asdasd3
asdasd4
asdasd6
What do you think ripgrep should have done?
Not duplicate the non-matching lines


---

_Label `duplicate` added by @BurntSushi on 2023-03-02 00:53_

---

_Comment by @BurntSushi on 2023-03-02 00:54_

Duplicate of #2095 and #2208. Was fixed in 91afd4214a9ada15e475bba02694f4fb50079cbc, which is on current master but has not yet been released.

---

_Closed by @BurntSushi on 2023-03-02 00:54_

---

_Comment by @elkarouh on 2023-03-02 06:52_

Hi BurntSushi,

When is a new release expected?


---

_Comment by @BurntSushi on 2023-03-02 12:48_

I don't schedule my free time. See: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#release

---

_Comment by @elkarouh on 2023-03-02 13:37_

No problem.
Your software is having massive impact on my productivity and i am just pushing the limits.
Many thanks for this wonderful tool !

---
