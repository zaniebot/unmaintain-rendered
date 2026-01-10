---
number: 448
title: Support macros in yaml
type: issue
state: closed
author: oars
labels: []
assignees: []
created_at: 2016-03-12T20:04:03Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/448
synced_at: 2026-01-10T01:26:29Z
---

# Support macros in yaml

---

_Issue opened by @oars on 2016-03-12 20:04_

Allow for use of crate_version! and the proposed crate_authors! when using yaml to build the app.


---

_Comment by @sru on 2016-03-12 20:55_

This is not possible as of now because even if loading YAML file is done in compile time, the YAML parsing is done in runtime (or so I believe, correct me if I'm wrong). What you could do, instead, is to add them after YAML parsing is done like so:

``` rust
let app = App::from_yaml(load_yaml!("some.yml"))
    .author(crate_authors!())
    .version(crate_version!());
```


---

_Comment by @kbknapp on 2016-03-13 00:11_

@sru that's correct. The actual parsing happens at runtime and just gives you a normal `App` instance that you can still mutate like normal as if it was created manually.

@oars although it's probably possible to special case those 2 particular macros, I'd rather just leave it to the method described above.

Does that method work for you, or do you something else?


---

_Comment by @oars on 2016-03-13 01:01_

@kbknapp That works perfecly.

Loving this lib. Thanks!


---

_Comment by @kbknapp on 2016-03-13 03:24_

:smile: :+1:


---

_Closed by @kbknapp on 2016-03-13 03:25_

---
