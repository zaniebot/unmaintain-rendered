```yaml
number: 20
title: Assertion in arg.rs makes multi-value options illegal?
type: issue
state: closed
author: archer884
labels: []
assignees: []
created_at: 2015-03-19T04:32:07Z
updated_at: 2018-08-02T03:29:36Z
url: https://github.com/clap-rs/clap/issues/20
synced_at: 2026-01-12T16:14:08Z
```

# Assertion in arg.rs makes multi-value options illegal?

---

_@archer884_

It's here: https://github.com/kbknapp/clap-rs/blob/master/src/args/arg.rs#L332

I am pretty new to the whole github thing--corporate work will do that to you!--so, if I'm doing this wrong, lemme know... I submitted a PR that fixes this, but, honestly, it's mostly just a proof of concept. There's no telling what errors it causes because I don't know what functions that assertion performed.

That said, it passes all the tests. :P


---

_Comment by @kbknapp on 2015-03-19 12:00_

Closed from PR #21 


---

_Closed by @kbknapp on 2015-03-19 12:00_

---
