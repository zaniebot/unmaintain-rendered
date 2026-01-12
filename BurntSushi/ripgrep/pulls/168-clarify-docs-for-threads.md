```yaml
number: 168
title: clarify docs for --threads
type: pull_request
state: merged
author: durka
labels: []
assignees: []
merged: true
base: master
head: doc-threads
created_at: 2016-10-11T21:33:16Z
updated_at: 2016-10-11T23:27:40Z
url: https://github.com/BurntSushi/ripgrep/pull/168
synced_at: 2026-01-12T18:23:12Z
```

# clarify docs for --threads

---

_@durka_

Fixes #167.


---

_Comment by @BurntSushi on 2016-10-11 22:28_

darwin builds take f..o..r..e..v..e..r..


---

_Comment by @durka on 2016-10-11 22:32_

Speaking of Darwin, `sed -i` works differently on OSX and [there's no real way to write it portably](http://stackoverflow.com/a/38595160/1114328), so `convert-to-man` doesn't work out of the box.


---

_Merged by @BurntSushi on 2016-10-11 23:27_

---

_Closed by @BurntSushi on 2016-10-11 23:27_

---

_Comment by @BurntSushi on 2016-10-11 23:27_

Screw it. Tired of waiting. Thanks!

(And yeah, I'm not particularly happy about that `convert-to-man` script for a number of reasons. Another problem for another day.)


---
