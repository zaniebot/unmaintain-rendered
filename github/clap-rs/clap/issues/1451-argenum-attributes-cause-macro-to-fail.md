---
number: 1451
title: "ArgEnum: attributes cause macro to fail"
type: issue
state: closed
author: ghost
labels:
  - C-enhancement
  - A-derive
assignees: []
created_at: 2019-04-07T21:45:18Z
updated_at: 2020-04-22T18:24:02Z
url: https://github.com/clap-rs/clap/issues/1451
synced_at: 2026-01-07T13:12:19-06:00
---

# ArgEnum: attributes cause macro to fail

---

_Issue opened by @ghost on 2019-04-07 21:45_

### Affected Version of clap
2.9.3

### Bug or Feature Request Summary

Right now, when using `arg_enum!` on an enum containing attributes, the compilation fails. 

```
arg_enum! {
    #[derive(Deserialize)]
    enum Foobar {
        #[serde(alias = "my_wonderful_alias")]
        HelloWorld
    }
}

#[serde(alias = "my_wonderful_alias")]
^ no rules expected this token in macro call
```

### Expected Behavior Summary

I think the `arg_enum` macro should accept those attributes, even if it does nothing with it. The current behaviour is quite limiting when using a single enum for both argument parsing, and actual processing (in my case, `actix_web::Query`).



---

_Comment by @alvisetrevisan on 2019-07-08 08:32_

I have a solution for this one, which is implemented in my fork https://github.com/alvisetrevisan/clap
I need to update the tests (they don't really check that the helper attributes are really parsed ok) and check a bit further, but this should work.

---

_Referenced in [clap-rs/clap#1547](../../clap-rs/clap/pulls/1547.md) on 2019-09-13 09:33_

---

_Label `clap-derive` added by @CreepySkeleton on 2020-02-01 14:39_

---

_Comment by @CreepySkeleton on 2020-02-01 14:40_

`arg_enum!` should eventually be replaced with a custom derive, so...

---

_Label `clap-derive-related` added by @CreepySkeleton on 2020-02-01 14:40_

---

_Comment by @pksunkara on 2020-02-01 20:10_

Arg enum related code is currently commented out in clap_derive. I haven't actually looked into it yet, but yes, it's a priority.

---

_Label `C: derive macros` added by @pksunkara on 2020-02-03 09:19_

---

_Label `clap-derive` removed by @pksunkara on 2020-02-03 09:19_

---

_Label `clap-derive-related` removed by @pksunkara on 2020-02-03 09:19_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-09 23:35_

---

_Label `T: enhancement` added by @pksunkara on 2020-04-11 14:53_

---

_Label `C: arg enums` added by @pksunkara on 2020-04-12 10:24_

---

_Closed by @bors[bot] on 2020-04-22 18:24_

---
