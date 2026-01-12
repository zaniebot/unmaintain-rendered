```yaml
number: 60
title: Add --literal search feature
type: issue
state: closed
author: martinlindhe
labels: []
assignees: []
created_at: 2016-09-24T11:23:03Z
updated_at: 2016-09-24T11:53:44Z
url: https://github.com/BurntSushi/ripgrep/issues/60
synced_at: 2026-01-12T18:23:11Z
```

# Add --literal search feature

---

_@martinlindhe_

This feature exists in ack and ag

```
ag:
       -Q --literal
              Do not parse PATTERN as a regular expression. Try to match  it
              literally.

ack:
       -Q, --literal
           Quote all metacharacters in PATTERN, it is treated as a literal.
```


---

_Comment by @martinlindhe on 2016-09-24 11:52_

Uhm, so. I found 

```

    -F, --fixed-strings        Treat the pattern as a literal string instead of
                               a regular expression.
```

Sorry for the noise!


---

_Closed by @martinlindhe on 2016-09-24 11:52_

---
