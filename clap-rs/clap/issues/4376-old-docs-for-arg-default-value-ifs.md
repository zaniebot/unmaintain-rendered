```yaml
number: 4376
title: "Old docs for `Arg::default_value_ifs`?"
type: issue
state: closed
author: zohnannor
labels: []
assignees: []
created_at: 2022-10-12T19:01:25Z
updated_at: 2022-10-12T19:12:16Z
url: https://github.com/clap-rs/clap/issues/4376
synced_at: 2026-01-12T16:14:15Z
```

# Old docs for `Arg::default_value_ifs`?

---

_@zohnannor_

[`Arg::default_value_ifs`](https://docs.rs/clap/latest/clap/builder/struct.Arg.html#method.default_value_ifs) docs says:

https://github.com/clap-rs/clap/blob/2926824d107f23cc654326b2d6261fcd94fec3f6/src/builder/arg.rs#L2780

which is false, I assume it was not updated after `v4`.

Another possibility: `ArgPredicate` is missing the `impl From<Option<&str>>`

---

_Closed by @epage on 2022-10-12 19:12_

---

_Comment by @epage on 2022-10-12 19:12_

Thanks for catching that and reporting it!

---
