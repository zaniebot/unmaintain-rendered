```yaml
number: 147
title: "the documentation for --replace should clarify that it doesn't write to files"
type: issue
state: closed
author: samuelcolvin
labels:
  - doc
assignees: []
created_at: 2016-10-04T12:28:04Z
updated_at: 2016-10-11T00:19:49Z
url: https://github.com/BurntSushi/ripgrep/issues/147
synced_at: 2026-01-12T18:23:11Z
```

# the documentation for --replace should clarify that it doesn't write to files

---

_@samuelcolvin_

(This time I have had read the README properly, at least I hope so)

The documentation in the readme doesn't explicitly say that `--replace` actually writes the replacement to file, but neither does it say the opposite. I would have assumed "Search and replace" meant write changes to file.

The current functionally appears to replace correctly in the output, but provide no way (I can see) to write those changes to file:

``` shell
samuel:~ || echo hello world > foo
samuel:~ || rg hello --replace "good bye" foo
1:good bye world
samuel:~ || cat foo
hello world
```

Same happens for recursive search and (don't) replace.

Again version 0.2.1 on ubuntu 16.04.


---

_Comment by @BurntSushi on 2016-10-04 12:32_

Indeed, it does not. It was requested to do in #74, but I'm pretty firmly oppposed.

A "search and replace" isn't really a standard option. Even a tool like `sed` won't do it without explicitly asking for it.

I agree the docs could be clarified.


---

_Renamed from "--replace doesn't write to files" to "the documentation for --replace should clarify that it doesn't write to files" by @BurntSushi on 2016-10-04 12:32_

---

_Comment by @samuelcolvin on 2016-10-04 12:48_

I've continued the feature request on #74, personally I find `sed` a complete mind f**k and as you say it's not a standard option of other commands which is why I'm so keen for it here.


---

_Label `bug` added by @BurntSushi on 2016-10-05 23:27_

---

_Label `doc` added by @BurntSushi on 2016-10-05 23:28_

---

_Label `bug` removed by @BurntSushi on 2016-10-05 23:28_

---

_Closed by @BurntSushi on 2016-10-11 00:19_

---
