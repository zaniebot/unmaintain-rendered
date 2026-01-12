```yaml
number: 443
title: specify and implement behavior for combining --only-matching with --replace
type: issue
state: closed
author: BurntSushi
labels:
  - question
assignees: []
created_at: 2017-04-10T10:40:23Z
updated_at: 2017-10-22T01:49:35Z
url: https://github.com/BurntSushi/ripgrep/issues/443
synced_at: 2026-01-12T16:13:22Z
```

# specify and implement behavior for combining --only-matching with --replace

---

_@BurntSushi_

@kpp writes:

How about:
```
$ rg '(?P<first>[A-Z][a-z]+)\s+(?P<last>[A-Z][a-z]+)'  tests/test.txt -o
7:Baker Street
```
```
$ rg '(?P<first>[A-Z][a-z]+)\s+(?P<last>[A-Z][a-z]+)'  tests/test.txt -o --replace '$last, $first'
7:Street, Baker
```

---

@mernen writes:

Guess someone beat me to implementing `-o`! (Which is fair, I’ve been postponing this since January)

So, as I mentioned in #308, I think the best combination for `--only-matching` and `--replace` would be an approximation of Ack’s `--output`: perform a replace on the matched part, and output only the result of the replacement, not anything else on the line. If there are multiple matches on a single line, output multiple lines. Same as on @kpp’s last comment, if I understood it correctly.

---

See also #422 

---

_Label `question` added by @BurntSushi on 2017-04-10 10:40_

---

_Comment by @BurntSushi on 2017-04-10 10:40_

Note that this still needs a specification. Ideally, the specification is in the form of **user facing documentation**.

---

_Comment by @BurntSushi on 2017-10-22 01:49_

This was done in #593.

---

_Closed by @BurntSushi on 2017-10-22 01:49_

---
