```yaml
number: 1517
title: Building with rustc 1.36.0 emits deprecation warnings.
type: issue
state: closed
author: nicodemus26
labels: []
assignees: []
created_at: 2019-07-05T19:10:13Z
updated_at: 2019-12-10T15:12:50Z
url: https://github.com/clap-rs/clap/issues/1517
synced_at: 2026-01-12T16:14:11Z
```

# Building with rustc 1.36.0 emits deprecation warnings.

---

_@nicodemus26_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

1.36.0

### Affected Version of clap

v2.33.0

### Bug or Feature Request Summary

Deprecation warnings when building examples, e.g. `cargo build --example 09_auto_version`
```
warning: use of deprecated item 'bitflags::core::str::<impl str>::trim_left_matches': superseded by `trim_start_matches`
  --> src/app/parser.rs:90:19
   |
90 |         let c = s.trim_left_matches(|c| c == '-')
   |                   ^^^^^^^^^^^^^^^^^ help: replace the use of the deprecated item: `trim_start_matches`
   |
   = note: #[warn(deprecated)] on by default

warning: use of deprecated item 'bitflags::core::str::<impl str>::trim_left_matches': superseded by `trim_start_matches`
  --> src/app/parser.rs:98:19
   |
98 |         let c = s.trim_left_matches(|c| c == '-')
   |                   ^^^^^^^^^^^^^^^^^ help: replace the use of the deprecated item: `trim_start_matches`

warning: use of deprecated item 'bitflags::core::str::<impl str>::trim_left_matches': superseded by `trim_start_matches`
   --> src/args/arg.rs:332:35
    |
332 |         self.s.short = s.as_ref().trim_left_matches(|c| c == '-').chars().nth(0);
    |                                   ^^^^^^^^^^^^^^^^^ help: replace the use of the deprecated item: `trim_start_matches`

warning: use of deprecated item 'bitflags::core::str::<impl str>::trim_left_matches': superseded by `trim_start_matches`
   --> src/args/arg.rs:372:30
    |
372 |         self.s.long = Some(l.trim_left_matches(|c| c == '-'));
    |                              ^^^^^^^^^^^^^^^^^ help: replace the use of the deprecated item: `trim_start_matches`

```

### Expected Behavior Summary

No deprecation warnings

---

_Referenced in [clap-rs/clap#1552](../../clap-rs/clap/issues/1552.md) on 2019-09-26 19:26_

---

_Comment by @cljoly on 2019-10-20 11:08_

When I build both `master` (daef001bbf32aeb29365832fa9c2fa327d8b1600) and `v3-master` (398b9dc6ccde0b8d43b023b215e0996134fd4718), I don’t get any warning. Maybe this issue has been solved and should be closed?

---

_Comment by @Dylan-DPC-zz on 2019-10-20 11:09_

Yeah we fixed it. thanks 

---

_Closed by @Dylan-DPC-zz on 2019-10-20 11:09_

---

_Comment by @faern on 2019-12-10 15:12_

Can there be a release with this fix? Since it's a macro the warning propagates to anyone using `clap`. It's been a persistent warning for quite a while now.

---
