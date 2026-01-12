```yaml
number: 1370
title: A flag to make --count return 0
type: issue
state: closed
author: cebaa
labels: []
assignees: []
created_at: 2019-09-08T13:38:47Z
updated_at: 2020-02-17T22:16:33Z
url: https://github.com/BurntSushi/ripgrep/issues/1370
synced_at: 2026-01-12T16:13:23Z
```

# A flag to make --count return 0

---

_@cebaa_

#### What version of ripgrep are you using?

ripgrep 11.0.1 (rev 973de50c9e)

#### How did you install ripgrep?

Github binary release.

#### What operating system are you using ripgrep on?

Linux.

#### Describe your question, feature request, or bug.

With `grep -cH needle app.log*`, you get something like:

```
app.log.2019-08-24:0
app.log.2019-08-25:3
app.log.2019-08-26:5
app.log.2019-08-27:0
```

which is visually useful for cases such as "interesting things happened on 25th and 26th, but not on 24th and 27th".

The difference is in the "not on 24th and 27th" part - without `rg` outputting zero counts it's not possible to distinguish between:

- No matches in those files
- Those files do not even exist
- You messed up a pattern and it's not matching files it should

It might be good to consider a flag to include zero counts as well. IMO it would help with sanity double-checks.

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
