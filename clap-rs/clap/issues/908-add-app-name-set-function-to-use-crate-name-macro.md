```yaml
number: 908
title: "Add App::name set function to use crate_name!() macro"
type: issue
state: closed
author: SuperFluffy
labels:
  - C-enhancement
  - A-builder
  - E-medium
assignees: []
created_at: 2017-03-20T16:02:00Z
updated_at: 2018-08-02T03:30:04Z
url: https://github.com/clap-rs/clap/issues/908
synced_at: 2026-01-12T16:14:10Z
```

# Add App::name set function to use crate_name!() macro

---

_@SuperFluffy_

Currently, you can further specify the `about`, `authors`, and `version` field of an `App` through setter methods of the same, and pull in the appropriate value through `crate_about!()` and similar macros. Only `crate_name!()` is special, in that we can only use it when constructing a new `App` through `App::new`.

If we construct an app through `App::from_yaml`, we have no way of setting the name to the one in Cargo.toml through the macro. It would be great to be able to set the name after `App` was constructed.

Is it possible to add this or is there a reason why this was omitted?

---

_Comment by @kbknapp on 2017-03-21 01:50_

It's not omiited on purpose,probably just overlooked. Until I get a chance to add this (I recently broke my finger so am typing with one hand and it's not feasible to get real work done 😞) there is a workaround (that is *technically* public, but its not meant to be public and is undocumented. As such it may change in the future without warning):

```rust
let mut app = // load YAML like normal
app.p.meta.name = "Some Name".into();
let m = app.get_matches();
```

If someone would like to submit a PR with this, it'd be an easy one just adding the fn to `src/app/mod.rs`!


---

_Label `C: app` added by @kbknapp on 2017-03-21 01:51_

---

_Label `D: easy` added by @kbknapp on 2017-03-21 01:51_

---

_Label `M: mentored` added by @kbknapp on 2017-03-21 01:51_

---

_Label `P3: want to have` added by @kbknapp on 2017-03-21 01:51_

---

_Label `T: enhancement` added by @kbknapp on 2017-03-21 01:51_

---

_Label `W: 2.x` added by @kbknapp on 2017-03-21 01:51_

---

_Comment by @SuperFluffy on 2017-03-21 10:08_

Sorry about your hand. :( To make it heal faster I submitted #910.

---

_Closed by @homu on 2017-03-22 00:27_

---

_Comment by @larsrh on 2018-02-13 20:32_

What I find a bit strange here is that I need to use a dummy name in the YAML file, because otherwise clap does some strange stuff:

```rust
        let mut a = if let Some(name) = yaml["name"].as_str() {
            App::new(name)
        } else {
            let yaml_hash = yaml.as_hash().unwrap();
            let sc_key = yaml_hash.keys().nth(0).unwrap();
            is_sc = Some(yaml_hash.get(sc_key).unwrap());
            App::new(sc_key.as_str().unwrap())
        };
```

---
