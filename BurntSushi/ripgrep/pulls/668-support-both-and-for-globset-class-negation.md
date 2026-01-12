```yaml
number: 668
title: "Support both [^...] and [!...] for globset class negation"
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: feature/glob-negation
created_at: 2017-11-09T04:34:08Z
updated_at: 2017-11-22T11:56:04Z
url: https://github.com/BurntSushi/ripgrep/pull/668
synced_at: 2026-01-12T18:23:13Z
```

# Support both [^...] and [!...] for globset class negation

---

_@okdana_

Fixes #663.

I hope this is the idiomatic way to do this. Seems to work at least:

```
% ls
abc  bbc  cbc  dbc

# Old behaviour
% \rg -u --files -g '[!ab]bc'
cbc
dbc
% \rg -u --files -g '[^ab]bc'
abc
bbc

# New behaviour
% rg-dev -u --files -g '[!ab]bc'
cbc
dbc
% rg-dev -u --files -g '[^ab]bc'
cbc
dbc
```

For the tests i just duplicated a bunch of the ones that were there for `[!...]` 🤷‍♀️

---

_Comment by @okdana on 2017-11-09 04:48_

Oops. Forgot to bump the `globset` version used by `ignore`. Pushing fix now....

Does the `ignore` version need changed too?

---

_@BurntSushi requested changes on 2017-11-16 22:14_

@okdana This looks great! The implementation itself is fine, and while I appreciate bumping the version, could you actually remove that part of the PR? I usually take care of version bumping whenever I do the next release. And yeah, `ignore` and `ripgrep` itself technically need semver bumps as well. (Which is fine.)

---

_Comment by @okdana on 2017-11-16 22:22_

Oh, sure. Done, rebased

---

_@BurntSushi approved on 2017-11-16 22:25_

Awesome thank you! :-)

---

_Merged by @BurntSushi on 2017-11-22 11:56_

---

_Closed by @BurntSushi on 2017-11-22 11:56_

---
