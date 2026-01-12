```yaml
number: 95
title: Implement Suggestions
type: issue
state: closed
author: kbknapp
labels: []
assignees: []
created_at: 2015-05-01T20:16:20Z
updated_at: 2018-08-02T03:29:39Z
url: https://github.com/clap-rs/clap/issues/95
synced_at: 2026-01-12T16:14:08Z
```

# Implement Suggestions

---

_@kbknapp_

This will be implemented via `feature` as it will incur an additional dep.

This is for spelling mistakes, for example if we had an application with `--file <file>` option and we ran

``` sh
$ myprog --flie words.txt
--flie isn't a valid argument for myprog
    Did you mean --file?

USAGE:
    myprog [FLAGS] [OPTIONS]
For more information try --help
```

Initial implementation will be longs and subcommnds only, but should be able to add values from `.possible_values()` later.


---

_Label `feature request` added by @kbknapp on 2015-05-01 20:16_

---

_Label `optional dep` added by @kbknapp on 2015-05-04 04:16_

---

_Comment by @kbknapp on 2015-05-05 13:19_

Currently being worked by @Byron in #103 


---

_Closed by @kbknapp on 2015-05-05 17:47_

---
