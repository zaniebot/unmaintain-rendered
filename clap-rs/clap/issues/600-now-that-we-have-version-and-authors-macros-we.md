---
number: 600
title: "Now that we have version and authors macros we should add App::new_with_defaults that uses both"
type: issue
state: closed
author: rtaycher
labels: []
assignees: []
created_at: 2016-07-24T10:24:00Z
updated_at: 2018-08-02T03:29:52Z
url: https://github.com/clap-rs/clap/issues/600
synced_at: 2026-01-10T01:26:33Z
---

# Now that we have version and authors macros we should add App::new_with_defaults that uses both

---

_Issue opened by @rtaycher on 2016-07-24 10:24_

I think it's generally best to make cmdline parsing as simple and short as possible.

In that spirit I'd like to propose adding a way to initialize version/authors
implicitly since this would add make it more difficult to build an app without cargo 
(although you could just define the 2 env variable manually) and since
App::new doesn't depend on it currently I thought people might be opposed to
changing it.

Instead how about adding a new method App::new_with_defaults?


---

_Comment by @rtaycher on 2016-07-24 10:24_

Implementation

```
pub fn new_with_defaults<S: Into<String>>(n: S) -> Self {
    App { p: Parser::with_name(n.into()) }.version(crate_version!()).author(crate_authors!())
}
```


---

_Label `T: RFC / question` added by @kbknapp on 2016-07-24 16:20_

---

_Comment by @kbknapp on 2016-07-24 16:22_

I think is a fine idea. I'd like to hear a few more opinions on the matter before adding it, primarily around the naming. I think `new_with_defaults` is kind of long. Maybe just `App::with_defaults(name)`? 


---

_Added to milestone `2.10.5` by @kbknapp on 2016-08-26 15:31_

---

_Closed by @kbknapp on 2016-08-28 03:42_

---
