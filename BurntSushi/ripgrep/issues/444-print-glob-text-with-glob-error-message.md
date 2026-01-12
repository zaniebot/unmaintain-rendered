```yaml
number: 444
title: Print glob text with glob error message
type: issue
state: closed
author: roblourens
labels:
  - enhancement
assignees: []
created_at: 2017-04-10T23:37:00Z
updated_at: 2017-04-12T22:16:25Z
url: https://github.com/BurntSushi/ripgrep/issues/444
synced_at: 2026-01-12T16:13:22Z
```

# Print glob text with glob error message

---

_@roblourens_

When I have an invalid glob:

```
$ rg -g "foo" -g "bar" -g "**.txt" foobar
invalid use of **; must be one path component

$ rg -g "foo" -g "bar" -g "{{}}" foobar
nested alternate groups are not allowed
```

There is an error printed, and the search doesn't continue, but it may not be obvious which glob is causing the issue. This comes up in https://github.com/Microsoft/vscode/issues/24050 but I imagine it may be useful for command line users to have a message like

`Invalid glob "{{}}", nested alternate groups are not allowed`

---

_Label `enhancement` added by @BurntSushi on 2017-04-10 23:37_

---

_Comment by @BurntSushi on 2017-04-10 23:38_

Fully agreed that the error message should be improved here. If it's just a matter of printing the invalid glob, that should hopefully be pretty easy to do. I'll see what I can do about this before the next release.

---

_Closed by @BurntSushi on 2017-04-12 22:14_

---

_Comment by @BurntSushi on 2017-04-12 22:16_

OK, I think this should be fixed. Here is the new output:

```
$ rg -g "foo" -g "bar" -g "**.txt" foobar
error parsing glob '**.txt': invalid use of **; must be one path component
$ rg -g "foo" -g "bar" -g "{{}}" foobar
error parsing glob '{{}}': nested alternate groups are not allowed
```

---
