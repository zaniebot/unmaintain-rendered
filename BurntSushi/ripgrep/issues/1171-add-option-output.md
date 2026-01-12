```yaml
number: 1171
title: Add OPTION --output
type: issue
state: closed
author: matjaz9
labels: []
assignees: []
created_at: 2019-01-22T13:41:13Z
updated_at: 2019-01-22T15:07:48Z
url: https://github.com/BurntSushi/ripgrep/issues/1171
synced_at: 2026-01-12T16:13:23Z
```

# Add OPTION --output

---

_@matjaz9_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Archives of precompiled binaries for ripgrep are available for Windows, macOS and Linux

#### What operating system are you using ripgrep on?

Windows 10

#### Describe your question, feature request, or bug.

This is proposal to introduce new switch --output. 
This switch is implemented on ACK utility and allows  output of variables together with some text. This allows composing of command files for process automation, etc..

Example of usage switch --output on ACK utility:
>ack "some text (\d{15})\s+(\d{11})" my_file.log --output "Command: MSISDN=$2, IMSI=$1;"

Drawback of using ACK on Windows is that perl needs to be installed  as well. Not everyone has it installed. While rg is compiled executable file and switch --output would make it very competitive, especially for Windows users.


---

_Comment by @BurntSushi on 2019-01-22 15:07_

This already exists. Please read the GUIDE and see the `-r/--replace` flag.

---

_Closed by @BurntSushi on 2019-01-22 15:07_

---
