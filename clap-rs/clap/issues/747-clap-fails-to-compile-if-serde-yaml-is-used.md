---
number: 747
title: "`clap` fails to compile if `serde_yaml` is used, because of `preserve_order` feature of `yaml-rust`"
type: issue
state: closed
author: emk
labels: []
assignees: []
created_at: 2016-11-13T17:06:01Z
updated_at: 2018-10-19T23:54:06Z
url: https://github.com/clap-rs/clap/issues/747
synced_at: 2026-01-10T01:26:34Z
---

# `clap` fails to compile if `serde_yaml` is used, because of `preserve_order` feature of `yaml-rust`

---

_Issue opened by @emk on 2016-11-13 17:06_

### Rust Version

rustc 1.14.0-nightly (cae6ab1c4 2016-11-05)

### Affected Version of clap

2.18.0 (and `serde_yaml` 0.5.0)

### Expected Behavior Summary

Clap should compile and link when `serde_yaml` 0.5.0 is being used in the same project.

### Actual Behavior Summary

When `serde_yaml` and `clap` are linked into the same executable, compilation fails with the following error:

```
error[E0308]: if and else have incompatible types
   --> /home/emk/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.18.0/src/args/group.rs:463:30
    |
463 |         let group_settings = if b.len() == 1 {
    |                              ^ expected struct `linked_hash_map::LinkedHashMap`, found struct `std::collections::BTreeMap`
    |
    = note: expected type `&linked_hash_map::LinkedHashMap<yaml_rust::Yaml, yaml_rust::Yaml>`
    = note:    found type `&'a std::collections::BTreeMap<yaml_rust::Yaml, yaml_rust::Yaml>`
```

The problem is that `serde_yaml` [configures `yaml-rust` to use the `preserve_order` feature](https://github.com/dtolnay/serde-yaml/blob/44456ddb34748fa2d40f565964961ff0515058c5/yaml/Cargo.toml#L17), which breaks `clap`, because `clap` doesn't expect the type of the exported `yaml-rust` APIs to change.

I'm going to file an issue with `serde_yaml` linking back here.

I'm not sure what the fix would be, but thank you for any help or advice you can provide!

---

_Referenced in [dtolnay/serde-yaml#33](../../dtolnay/serde-yaml/issues/33.md) on 2016-11-13 17:08_

---

_Referenced in [chyh1990/yaml-rust#44](../../chyh1990/yaml-rust/issues/44.md) on 2016-11-13 17:42_

---

_Comment by @Prior99 on 2016-11-14 20:18_

+1 as I am facing the exact same problem. Both clap and serde are really popular libraries which makes this a huge problem. I would be happy to help but have honestly no idea on where to start...


---

_Comment by @emk on 2016-11-14 20:32_

@Prior99: Try downgrading your `serde_yaml` to 0.4.1 as a temporary workaround while this gets sorted out. That version is compatible with `clap`.


---

_Comment by @kbknapp on 2016-11-14 20:48_

Catching up on issues. Thanks for reporting this, I'm looking into it as well! :+1: 


---

_Label `T: RFC / question` added by @kbknapp on 2016-11-14 20:48_

---

_Comment by @kbknapp on 2016-11-14 21:05_

Ok, so I've read all the related issues and unfortunately changing clap's API to work with the `preserve_order` feature is a breaking change. So for now, pinning to `serde_yaml` 0.4.1 is the only workaround.

I've been toying with the idea of a 3.x for a while now, at which case I _could_ change clap's API to match the `preserve_order` feature.

This is a very sticky situation, and I tend to agree with the cargo maintainers that features should be additive only, and not change the API at all. It also seems to me if `serde_yaml` had an option to not use the `preserve_order` feature that would solve this as well. But I don't think it's possible to selectively turn features on/off with your own features (i.e. have the same dependencies, but with different features depending on what's passed to your crate).


---

_Comment by @Prior99 on 2016-11-14 21:09_

I have a fix which I will publish after some testing. It basically uses LinkedHashMap instead of BTree for the yaml feature of clap. 


---

_Comment by @kbknapp on 2016-11-14 21:12_

@Prior99 I haven't looked in depth yet, since it's been quite a while since I wrote the YAML parts of clap, but I _think_ you can't do that without changing the public API, making it a breaking change. I'm open to seeing what you've got though! :smile: 


---

_Comment by @Prior99 on 2016-11-14 21:21_

Couldn't this be fixed by #750? All the tests passed for me, but please make sure this works by integrating it somewhere. I currently have nothing ready to do so on my own, **if** this would be an acceptable fix.


---

_Referenced in [clap-rs/clap#750](../../clap-rs/clap/pulls/750.md) on 2016-11-15 13:24_

---

_Referenced in [estk/log4rs#35](../../estk/log4rs/issues/35.md) on 2016-12-13 18:59_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-08-22 04:17_

---

_Referenced in [clap-rs/clap#1110](../../clap-rs/clap/issues/1110.md) on 2017-11-22 09:16_

---

_Comment by @igor-raits on 2017-11-22 19:54_

@kbknapp yaml-rust 0.4 has been released and now it uses only linked-hash-map, no more btreemap. Should not we change that too?

---

_Comment by @kbknapp on 2017-11-27 15:12_

@ignatenkobrain I'm not against it...however that is technically a breaking change to the public API and any code using BTreeMap (or any code using `Arg::from_yaml`) will break. I'd very much like to hear from some of the YAML users as to if this breaks anything for them (as not many people should be using `Arg::from_yaml` directly, it was meant to be only be used internally by `clap` when building out an entire YAML CLI.

---

_Comment by @rrichardson on 2018-01-12 01:51_

~~FWIW -  I am running 2.29.1 and `cargo tree` tells me that my only linked yaml dependency is `yaml-rust : 0.3.5`~~ 
~~Loading via  `App::from_yaml(yaml);` fails in `Arg::from_yaml`.  It takes the `nth(0)` key and attempts to coerce it via `as_hash()` which fails, because the value is just `String(...)`
This *might* be the ordering problem mentioned,  but nothing in the entire vec that it is looping through has a yaml type which could be coerced via `as_hash()`~~~

Edit: 

This was my mistake.. my editor auto-formatted the yaml in an incorrect way and I was not indenting each arg  under the array to become a map.  

---

_Added to milestone `serde` by @kbknapp on 2018-02-02 01:55_

---

_Removed from milestone `serde` by @kbknapp on 2018-02-02 01:55_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-02 01:55_

---

_Comment by @kbknapp on 2018-07-22 00:59_

This has been fixed on v3-master

---

_Closed by @kbknapp on 2018-07-22 00:59_

---

_Comment by @ghempton on 2018-10-19 23:54_

I see that v3-master hasn't been updated in a while and I get a compile error if I try and reference it in my `cargo.toml`:

```
error[E0308]: mismatched types
   --> /Users/ghempton/.cargo/git/checkouts/clap-78dbe9b58f9073fe/eaa0700/src/build/macros.rs:82:9
    |
82  | /         $v.as_str()
83  | |             .unwrap_or_else(|| panic!("failed to convert YAML {:?} value to a string", $v))
    | |___________________________________________________________________________________________^ expected char, found &str
    |
   ::: /Users/ghempton/.cargo/git/checkouts/clap-78dbe9b58f9073fe/eaa0700/src/build/arg/mod.rs:166:28
    |
166 |                   "short" => yaml_to_str!(a, v, short),
    |                              ------------------------- in this macro invocation
    |
    = note: expected type `char`
               found type `&str`
```

Are there any plans to push v3 forward or get this back-ported into v2?

---
