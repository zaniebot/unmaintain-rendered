```yaml
number: 1117
title: Early return / shortened output when reading from stdin with binary garbage in the middle of file
type: issue
state: closed
author: vorner
labels:
  - duplicate
assignees: []
created_at: 2018-11-22T09:32:59Z
updated_at: 2018-11-29T17:58:02Z
url: https://github.com/BurntSushi/ripgrep/issues/1117
synced_at: 2026-01-12T16:13:22Z
```

# Early return / shortened output when reading from stdin with binary garbage in the middle of file

---

_@vorner_

#### What version of ripgrep are you using?

0.10.0

#### How did you install ripgrep?

By cargo install.

#### What operating system are you using ripgrep on?

Gentoo linux (it doesn't have a „version“, it has rolling updates ‒ but I keep it up to date). Kernel 4.19.

#### Describe your question, feature request, or bug.

I have a biggish log file (310MB). When I run rg like this:

```
rg 74.125.71.189 log.txt
```

I get several pages of output (5769 lines).

If I put the data onto its stdin:

```
rg 74.125.71.189 <log.txt
```

I get only the first 12 lines of output. It's always 12. If I pipe it from other command, the same happens.

If using grep, it always prints the 12 lines and then `Binary file (standard input) matches`.

I believe that:
* Both modes (file as a parameter and from stdin) should act consistently. This is surprising.
* If rg gives up because it finds some binary garbage, it should let me know *somehow* ‒ either by an error message like that, or additionaly by actually terminating with non-zero error code. This way I could have assumed the log file contained only the 12 matches, which is misleading.

Adding the `-a` parameter makes it print all the lines.

Sorry, I can't share the input file :-(. But I hope the description is clear enough without it.

Running with `--debug` only prepends the output with these two lines:

```
DEBUG|grep_regex::literal|/home/vorner/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-regex-0.1.1/src/literal.rs:110: required literal found: "189"
DEBUG|globset|/home/vorner/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.2/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```




---

_Comment by @BurntSushi on 2018-11-22 12:47_

I'm on mobile so I can't find the issue at the moment, but this is unfortunately a known bug. I agree that they should have the same behavior. The particular reason is that reason from stdin will do binary detection everywhere, where as searching a file could use memory maps, which only does binary detection on the first few KB of the file. This leads to a difference in behavior.

I'm still trying to figure out how to overhaul binary detection. One thing you can do if you want consistent behavior is add the `--no-mmap` flag to your ripgrep config file.

---

_Comment by @vorner on 2018-11-24 19:49_

Anyway, some kind of „Binary file, giving up“ would be nice, so I at least know it's not the complete output.

---

_Comment by @BurntSushi on 2018-11-29 17:57_

This is a duplicate of #306. I've added a comment there explaining more about what the next steps might be.

---

_Closed by @BurntSushi on 2018-11-29 17:57_

---

_Label `duplicate` added by @BurntSushi on 2018-11-29 17:58_

---
