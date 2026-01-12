```yaml
number: 1244
title: Report count of each match
type: issue
state: closed
author: sagis-tikal
labels:
  - wontfix
assignees: []
created_at: 2019-04-14T12:20:46Z
updated_at: 2019-04-14T12:53:12Z
url: https://github.com/BurntSushi/ripgrep/issues/1244
synced_at: 2026-01-12T16:13:23Z
```

# Report count of each match

---

_@sagis-tikal_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)


#### How did you install ripgrep?

Installed using the following method
$ curl -LO https://github.com/BurntSushi/ripgrep/releases/download/0.10.0/ripgrep_0.10.0_amd64.deb
$ sudo dpkg -i ripgrep_0.10.0_amd64.deb

#### What operating system are you using ripgrep on?

Ubuntu 14.04 - 64-bit

#### Describe your question, feature request, or bug.

I'm trying to query a lot of log files (some archived and some aren't) using a list of ~10K patterns (using the "-f" flag).
I would like ripgrep to generate a report of how many times each pattern was found in all of the log files.


Thank you Andrew, you are an awesome developer :D


---

_Comment by @BurntSushi on 2019-04-14 12:53_

It's an interesting use case, but I'm going to have to decline. Despite the seeming simplicity of the request, this is something that would require a fair amount of work to design correctly, and would add yet another type of output mode to ripgrep, which I'm not keen on adding.

I would instead suggest that you use ripgrep's libraries and write a Rust program to achieve what you want.

---

_Closed by @BurntSushi on 2019-04-14 12:53_

---

_Label `wontfix` added by @BurntSushi on 2019-04-14 12:53_

---
