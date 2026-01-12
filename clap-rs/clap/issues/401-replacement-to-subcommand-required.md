```yaml
number: 401
title: "Replacement to `subcommand_required`?"
type: issue
state: closed
author: untitaker
labels: []
assignees: []
created_at: 2016-01-29T21:32:30Z
updated_at: 2018-08-02T03:29:47Z
url: https://github.com/clap-rs/clap/issues/401
synced_at: 2026-01-12T16:14:09Z
```

# Replacement to `subcommand_required`?

---

_@untitaker_

The README lists all the deprecated methods, but doesn't suggest a migration path. I suspect I'm supposed to handle this case with my own program logic?


---

_Comment by @kbknapp on 2016-01-30 06:33_

My mistake for not listing it. There is an `AppSettings` variant that takes care of it.

``` rust
use clap::{App, SubCommand, AppSettings};

App::new("test")
    .setting(AppSettings::SubcommandRequired)
    .subcommand(SubCommand::with_name("stuff"));
```


---

_Label `T: RFC / question` added by @kbknapp on 2016-01-30 06:33_

---

_Closed by @kbknapp on 2016-01-31 19:32_

---

_Comment by @untitaker on 2016-01-31 20:35_

Thank you!


---
