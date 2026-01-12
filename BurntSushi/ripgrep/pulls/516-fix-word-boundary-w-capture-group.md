```yaml
number: 516
title: fix word boundary w/ capture group
type: pull_request
state: merged
author: emattiza
labels: []
assignees: []
merged: true
base: master
head: 506-emattiza-parentheses-boundary-search
created_at: 2017-06-15T03:16:05Z
updated_at: 2017-08-01T01:21:31Z
url: https://github.com/BurntSushi/ripgrep/pull/516
synced_at: 2026-01-12T18:23:13Z
```

# fix word boundary w/ capture group

---

_@emattiza_

fixes BurntSushi/ripgrep#506. Word boundary search as arg had unexpected
behavior. added non-capturing capture group to regex to encapsulate 'or' option search and
prevent expansion and partial boundary finds.

Signed-off-by: Evan.Mattiza <emattiza@gmail.com>

---

_Merged by @BurntSushi on 2017-06-15 10:55_

---

_Closed by @BurntSushi on 2017-06-15 10:55_

---

_Comment by @BurntSushi on 2017-06-15 10:56_

Awesome, thanks so much! :-)

---

_Branch deleted on 2017-08-01 01:21_

---
