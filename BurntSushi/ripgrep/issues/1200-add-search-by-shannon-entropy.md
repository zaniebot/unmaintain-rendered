```yaml
number: 1200
title: Add search by shannon entropy
type: issue
state: closed
author: xstevens
labels:
  - question
assignees: []
created_at: 2019-02-19T01:21:45Z
updated_at: 2019-02-19T04:56:55Z
url: https://github.com/BurntSushi/ripgrep/issues/1200
synced_at: 2026-01-12T16:13:23Z
```

# Add search by shannon entropy

---

_@xstevens_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Homebrew

#### What operating system are you using ripgrep on?

macOS

#### Describe your question, feature request, or bug.

I thought it would be really handy if ripgrep supported shannon entropy search. By that I mean adding an option, so as it's searching through text; if any given word has entropy above a threshold then display it as a match. I currently use other tools for this type of search, though I use ripgrep for regex and most other types of grep functionality. It would be handy and I suspect faster to have all of that in this tool.


---

_Comment by @BurntSushi on 2019-02-19 01:33_

This doesn't sound like a good fit for ripgrep, sorry. If speed is important, you might consider reusing some of ripgrep's libraries for this. It shouldn't be too much code for such a specialized use case. High level documentation for ripgrep's libraries is still missing, but [this example](https://github.com/BurntSushi/ripgrep/blob/master/grep/examples/simplegrep.rs) should be a good starting point for now.

---

_Closed by @BurntSushi on 2019-02-19 01:33_

---

_Label `question` added by @BurntSushi on 2019-02-19 01:33_

---

_Comment by @xstevens on 2019-02-19 04:56_

I understand. Thanks for the example!

---
