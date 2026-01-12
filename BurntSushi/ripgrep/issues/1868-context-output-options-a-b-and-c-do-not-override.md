```yaml
number: 1868
title: "Context output options (-A, -B, and -C) do not override the \"--passthru\" option"
type: issue
state: closed
author: lousyd
labels:
  - bug
  - rollup
assignees: []
created_at: 2021-05-19T15:47:59Z
updated_at: 2021-06-01T01:51:19Z
url: https://github.com/BurntSushi/ripgrep/issues/1868
synced_at: 2026-01-12T16:13:24Z
```

# Context output options (-A, -B, and -C) do not override the "--passthru" option

---

_@lousyd_

#### What version of ripgrep are you using?

ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

dnf install ripgrep

#### What operating system are you using ripgrep on?

GNU/Linux 5.11.18-300.fc34.x86_64 x86_64

#### Describe your bug.

The general approach of configuration and command line options seems to be that if two options conflict, the later one prevails and the earlier one is treated as if it were never given. For example, the "--binary" option will cancel out the "--no-binary" option if it appears later in the configuration file or later on the command line. Similarly, "--text" will override the "--binary" option if it appears later in the configuration file or command line, and vice versa. However, the context output options (-A, -B, and -C) do not similarly override and get overridden by the "--passthru" option. It seems as if they should override each other.

I encountered this when spitting out a bunch of JSON that I wanted to filter through ripgrep. I put together my command line to output the JSON, then piped it to rg with "--passthru" and my pattern. It worked as expected. Then I up-arrowed to get the same command line but added a "-C" to it. I didn't even think about the fact that I hadn't removed the previous "--passthru". I just thought, conceptually, that I was further refining the command line to filter down the results a little more. It confused me when I still got aaalllll of the JSON.

#### What are the steps to reproduce the behavior?

```
printf 'line 1\nline 2\nline 3\n' | rg --passthru -B1 3
printf 'line 1\nline 2\nline 3\n' | rg -B1 --passthru 3
```

#### What is the expected behavior?

It seems as if the context output options (-A, -B, and -C) should override the "--passthru" option, and vice versa, when given later in the configuration file or command line.


---

_Comment by @BurntSushi on 2021-05-19 15:51_

Yes, I think I agree.

---

_Label `bug` added by @BurntSushi on 2021-05-19 15:51_

---

_Label `rollup` added by @BurntSushi on 2021-05-31 01:43_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
