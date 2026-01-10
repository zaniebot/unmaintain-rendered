---
number: 6129
title: "feat: Add default_values_if and default_values_ifs"
type: pull_request
state: merged
author: cooronx
labels: []
assignees: []
merged: true
base: master
head: feat_default_values_if_and_default_values_ifs
created_at: 2025-09-13T14:22:46Z
updated_at: 2025-11-19T20:49:38Z
url: https://github.com/clap-rs/clap/pull/6129
synced_at: 2026-01-10T01:28:27Z
---

# feat: Add default_values_if and default_values_ifs

---

_Pull request opened by @cooronx on 2025-09-13 14:22_

Fixes #5698 

---

_@epage reviewed on 2025-09-15 13:57_

---

_Review comment by @epage on `clap_builder/src/builder/arg.rs`:3071 on 2025-09-15 13:57_

`default_values` doesn't take `IntoRessettable`.  Likely this shouldn't as well.

---

_Review comment by @epage on `clap_builder/src/builder/arg.rs`:3216 on 2025-09-15 13:57_

ditto

---

_@epage reviewed on 2025-09-15 13:57_

---

_@epage reviewed on 2025-09-15 13:57_

---

_Review comment by @epage on `tests/builder/default_vals.rs`:319 on 2025-09-15 13:57_

If the tests aren't being added before the commit, could you add them in the same commit?

---

_@cooronx reviewed on 2025-09-15 14:19_

---

_Review comment by @cooronx on `tests/builder/default_vals.rs`:319 on 2025-09-15 14:19_

got it, i will open a new PR with tests before feature

---

_@cooronx reviewed on 2025-09-15 14:19_

---

_Review comment by @cooronx on `clap_builder/src/builder/arg.rs`:3071 on 2025-09-15 14:19_

got it, i will fix it

---

_@cooronx reviewed on 2025-09-15 14:48_

---

_Review comment by @cooronx on `clap_builder/src/builder/arg.rs`:3071 on 2025-09-15 14:48_

@epage I have some doubts about the implementation
```
Arg::new("args")
       .long("args")
       .num_args(2)
       .default_values_if("opt", "value", [Some("df1".into()),None]) // the first arg set to df1, second arg set to None 
```
Do you think this usage is allowed?
or just 
```
Arg::new("args")
       .long("args")
       .num_args(2)
       .default_values_if("opt", "value", None) // means two args all set to None
```
I personally think the second one is better

---

_Review comment by @cooronx on `clap_builder/src/builder/arg.rs`:3071 on 2025-09-16 09:19_

@epage sorry for the trouble, I checked the `default_values` and i find out `default_values` doesn't take `None`, so the correct version of default_values_if should be
```
Arg::new("args")
       .long("args")
       .num_args(2)
       .default_values_if("opt", "value",[] ) // means two args all set to None
```
not sure if it's correct usage

---

_@cooronx reviewed on 2025-09-16 09:19_

---

_@epage reviewed on 2025-09-16 18:59_

---

_Review comment by @epage on `clap_builder/src/builder/arg.rs`:3071 on 2025-09-16 18:59_

Yes, that would match `default_values`

---

_@epage reviewed on 2025-09-16 18:59_

---

_Review comment by @epage on `tests/builder/default_vals.rs`:319 on 2025-09-16 18:59_

Doesn't need to be a new one, feel free to force-push to this one

---

_@epage reviewed on 2025-09-17 13:54_

---

_Review comment by @epage on `tests/builder/default_vals.rs`:319 on 2025-09-17 13:54_

My "tests added before the commit" was referring to an encouraged practice from our [CONTRIBUTING.md](https://github.com/clap-rs/clap/blob/master/CONTRIBUTING.md#preparing-the-pr) but more importantly, commits should be atomic.  In this case, the test commit will fail to build because it uses APIs not yet available.  In a situation like this, the practice from the CONTRIB doesn't quite make sense, so the tests would be in the same commit as the change which is what I was asking for though I can see how my statement could be taken as preferring that more.

---

_@cooronx reviewed on 2025-09-17 14:11_

---

_Review comment by @cooronx on `tests/builder/default_vals.rs`:319 on 2025-09-17 14:11_

You're right, this kind of commit will cause build fail 😿. sorry for the misunderstanding

---

_@epage reviewed on 2025-11-17 15:32_

---

_Review comment by @epage on `clap_builder/src/builder/arg.rs`:84 on 2025-11-17 15:32_

Why is this `Vec<Option<OsStr>>` rather than `Option<Vec<OsStr>>`?

---

_@epage reviewed on 2025-11-17 15:32_

---

_Review comment by @epage on `clap_builder/src/builder/arg.rs`:3109 on 2025-11-17 15:32_

Why are we treating an empty value is a sentinel value?

---

_@cooronx reviewed on 2025-11-18 15:55_

---

_Review comment by @cooronx on `clap_builder/src/builder/arg.rs`:3109 on 2025-11-18 15:55_

@epage I'm a little confused about this.
1. `pub fn default_values_if(
        mut self,
        arg_id: impl Into<Id>,
        predicate: impl Into<ArgPredicate>,
        defaults: impl IntoIterator<Item = impl Into<OsStr>>,
    )` in order to match `pub fn default_values(mut self, vals: impl IntoIterator<Item = impl Into<OsStr>>)`
2. however, If written this way, it will prevent the input of `None`.
3. to represent the value `None`, I have to write it like this.


```
default_values_if("opt", "value", None)  // error, default_values doesn't take IntoRessettable
default_values_if("opt", "value", []) // error, type annotations needed


default_values_if("opt", "value", [""]) // ok, one way to express None, but if user just want a empty string? And we need to treat an empty value as a sentinel value
```


So I'm not quite sure how to implement it. Maybe `None` is not allowed in default_values_if?

---

_@epage reviewed on 2025-11-18 17:15_

---

_Review comment by @epage on `clap_builder/src/builder/arg.rs`:3109 on 2025-11-18 17:15_

If a user needs `None`, they should likely be using `default_value_if`.

---

_@epage reviewed on 2025-11-18 17:15_

---

_Review comment by @epage on `clap_builder/src/builder/arg.rs`:3109 on 2025-11-18 17:15_

Just like `default_value` / `default_values`

---

_@cooronx reviewed on 2025-11-18 17:37_

---

_Review comment by @cooronx on `clap_builder/src/builder/arg.rs`:3109 on 2025-11-18 17:37_

got it, thx

---

_@cooronx reviewed on 2025-11-19 12:38_

---

_Review comment by @cooronx on `clap_builder/src/builder/arg.rs`:84 on 2025-11-19 12:38_

fixed

---
