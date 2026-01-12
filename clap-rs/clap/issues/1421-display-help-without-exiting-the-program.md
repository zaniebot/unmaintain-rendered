```yaml
number: 1421
title: Display Help without exiting the program.
type: issue
state: closed
author: omarabid
labels: []
assignees: []
created_at: 2019-02-23T17:12:56Z
updated_at: 2019-07-17T18:41:39Z
url: https://github.com/clap-rs/clap/issues/1421
synced_at: 2026-01-12T16:14:10Z
```

# Display Help without exiting the program.

---

_@omarabid_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

* rustc 1.32.0 (9fda7c223 2019-01-16)

### Affected Version of clap

* clap 2.32.0

### Bug or Feature Request Summary

.setting(AppSettings::ArgRequiredElseHelp)

This option/setting exit the application.

### Expected Behavior Summary

Carry on running code or provide an option that displays help and carries on execution.

### Sample Code or Link to Sample Code

Here is the current solution that I have found working: https://stackoverflow.com/questions/54837057/display-help-with-clap-after-get-matches

You need to get the help before matching, store it in some variable and then display it later.


---

_Comment by @omarabid on 2019-02-23 17:14_

ArgMatches do provide a "usage" function (https://kbknapp.github.io/clap-rs/clap/struct.ArgMatches.html#method.usage) but this doesn't display the whole/long help text. Maybe it could be changed or another function added.

---

_Closed by @omarabid on 2019-07-17 18:41_

---
