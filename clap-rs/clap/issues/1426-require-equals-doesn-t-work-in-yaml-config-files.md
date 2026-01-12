```yaml
number: 1426
title: "\"require_equals\" doesn't work in YAML config files"
type: issue
state: closed
author: rivy
labels: []
assignees: []
created_at: 2019-03-11T03:31:21Z
updated_at: 2019-07-22T00:35:45Z
url: https://github.com/clap-rs/clap/issues/1426
synced_at: 2026-01-12T16:14:10Z
```

# "require_equals" doesn't work in YAML config files

---

_@rivy_

### Rust Version

`rustc 1.34.0-nightly (097c04cf4 2019-02-24)`

### Affected Version of clap

```
name = "clap"
"clap 2.32.0 (registry+https://github.com/rust-lang/crates.io-index)",
"checksum clap 2.32.0 (registry+https://github.com/rust-lang/crates.io-index)" = "b957d88f4b6a63b9d70d5f454ac8011819c6efa7727858f458ab71c756ce2d3e"
```

### Bug or Feature Request Summary

"require_equals" within YAML configurations causes build errors:

`... panicked at 'Unknown Arg setting 'require_equals' in YAML file ...`

---

_Comment by @rivy on 2019-07-22 00:35_

Fixed in v3 by #1508 (merged).

The workaround for v2 is to use yaml input as usual for the majority of options and then add on the options manually for those needing `require_equals()` ...

```js
use clap::App;
let yaml = load_yaml!( ... );
let matches = App::from_yaml(yaml)
        .arg(clap::Arg::with_name("color").long("color")
            .default_value("auto")
            .global(true)
            .value_name("WHEN")
            .multiple(true)
            .min_values(0)
            .require_equals(true)
            .overrides_with("color")
            )
        .version(crate_version!())
        .set_term_width(90)
        .setting(AppSettings::SubcommandRequired)
        .get_matches();
```

---

_Closed by @rivy on 2019-07-22 00:35_

---
