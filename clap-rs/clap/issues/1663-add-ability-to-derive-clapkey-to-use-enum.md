```yaml
number: 1663
title: Add ability to derive ClapKey to use enum variants as keys
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-builder
  - A-derive
  - S-wont-fix
assignees: []
created_at: 2017-11-12T20:12:40Z
updated_at: 2022-05-03T14:13:13Z
url: https://github.com/clap-rs/clap/issues/1663
synced_at: 2026-01-12T16:14:11Z
```

# Add ability to derive ClapKey to use enum variants as keys

---

_@kbknapp_

Maintainer's notes:
- This presupposes enum variants, so this is bocked on #1104
---
See related discussion in kbknapp/clap-rs#1104

Once the details in that issue are hashed out, we could then add support for:

```rust
#[derive(ClapKey)]
enum MyArgs {
     Foo,
     Bar,
     Baz,
}

fn main() {
    let matches = App::new("")
        /* stuff */ 
        .get_matches();
    let value = matches.value_of(MyArgs::Foo);
}
```

---

_Comment by @Hoverbear on 2017-11-12 20:28_

Should this be part of initial publish?

---

_Comment by @kbknapp on 2017-11-12 20:44_

This brings up a bigger point. Should clap-derives be usable on clap 2.x?

I'm leaning yes, because what you've done already works today. I *also* think we should be pushing towards a clap-derives which works on clap 3.x which this issue is aimed at.

So I think we need to find a way to differentiate between these two branches.

---

_Label `C: args` added by @pksunkara on 2020-02-03 09:33_

---

_Added to milestone `3.0` by @pksunkara on 2020-02-03 09:33_

---

_Label `C: derive macros` added by @CreepySkeleton on 2020-02-03 09:39_

---

_Label `C: arg enums` added by @pksunkara on 2020-04-12 10:28_

---

_Referenced in [clap-rs/clap#1104](../../clap-rs/clap/issues/1104.md) on 2021-07-20 14:01_

---

_Removed from milestone `3.0` by @pksunkara on 2021-07-25 14:05_

---

_Added to milestone `4.0` by @pksunkara on 2021-07-25 14:05_

---

_Referenced in [epage/clapng#135](../../epage/clapng/issues/135.md) on 2021-12-06 19:11_

---

_Label `C: arg enums` removed by @epage on 2021-12-08 19:54_

---

_Label `C: args` removed by @epage on 2021-12-08 19:54_

---

_Label `A-builder` added by @epage on 2021-12-08 19:54_

---

_Label `C-enhancement` added by @epage on 2021-12-09 17:11_

---

_Label `S-blocked` added by @epage on 2021-12-09 17:11_

---

_Removed from milestone `4.0` by @epage on 2021-12-09 17:11_

---

_Comment by @epage on 2022-02-02 20:56_

With #1104 closed, I'm closing this as well.

---

_Closed by @epage on 2022-02-02 20:56_

---

_Label `S-blocked` removed by @epage on 2022-05-03 14:13_

---

_Label `S-wont-fix` added by @epage on 2022-05-03 14:13_

---
