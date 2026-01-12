```yaml
number: 2742
title: Include regex syntax in man page
type: issue
state: open
author: tournemire
labels:
  - question
assignees: []
created_at: 2024-02-22T17:10:34Z
updated_at: 2024-02-22T17:13:48Z
url: https://github.com/BurntSushi/ripgrep/issues/2742
synced_at: 2026-01-12T16:13:24Z
```

# Include regex syntax in man page

---

_@tournemire_

Ripgrep's manpage does not document its default regex syntax, instead redirecting to the regex crate's documentation. This makes offline use with non-trivial regexes impractical, as end-users do not necessarily have a local copy of the crate's doc.

---

_Comment by @BurntSushi on 2024-02-22 17:13_

I think this has been requested before, but duplicating the regex syntax doc is quarrelsome for a couple reasons:

1. It is extremely long. It would be too long to include it in `man rg` IMO. Which means we'd probably need to ship a `man rg-regex-syntax` or something.
2. This would create two separate copies of the syntax documentation, which in turn means it'd be easy for them to get out-of-sync with one another.
3. The regex syntax doc is "plain" HTML/CSS/some-javascript. It is not hard to download it if you need an offline copy.

---

_Label `question` added by @BurntSushi on 2024-02-22 17:13_

---
