---
number: 1837
title: Implement update_from_arg_matches()
type: issue
state: closed
author: lu-zero
labels: []
assignees: []
created_at: 2020-04-17T08:29:34Z
updated_at: 2020-11-14T11:38:17Z
url: https://github.com/clap-rs/clap/issues/1837
synced_at: 2026-01-10T01:27:08Z
---

# Implement update_from_arg_matches()

---

_Issue opened by @lu-zero on 2020-04-17 08:29_

`clap_derive` has only methods to create a new structure from scratch.

Provide a set of methods that can use ArgMatches to update a previously instantiated struct 

``` rust
trait FromArgMatches {
    fn update_from_arg_matches(&mut self, matches: &ArgMatches);
    ...
}
```

From the [discussion](https://github.com/clap-rs/clap/discussions/1836):
> 
> INPUTS:
> 
>  * An old struct already filled by the derive. This struct needs to be updated.
> 
>  * An `ArgMatches` instance, the source of updated values.
> 
> 
> STEPS:
> 
> * For every field in the struct:
>    * Check if the updated value is available in `ArgMatches`. If yes, convert value to the field's type and assign the field with the new value (i.e `self.field = matches.value_of("field_id").convert().`). If no, do nothing (i.e, leave the old value in place).
>    * For flattened fields and subcommands, delegate to corresponding structs recursively (easy enough).
> 

---

_Label `T: new feature` added by @lu-zero on 2020-04-17 08:29_

---

_Comment by @CreepySkeleton on 2020-04-17 09:41_

A note for other maintainers: this is not the same https://github.com/clap-rs/clap/issues/748 , this one is very derive-centric.

This is about updating a preexisting `#[derive(Clap)]` struct from a preexisting `ArgMatches` instance, not about updating a preexisting `ArgMatches` from \<whatever>

---

_Comment by @CreepySkeleton on 2020-04-28 10:38_

@lu-zero Would you be interested in implementing this?

---

_Comment by @lu-zero on 2020-04-28 14:53_

I would, I will need probably some code tour on what's already available though.

---

_Comment by @CreepySkeleton on 2020-04-28 18:49_

@lu-zero Basically, you need to add 
```rust
fn update_from_arg_matches(&mut self, matches: &ArgMatches);
```

into every trait except `IntoApp`. You can start from here: open a PR and I'll answer any of your questions regarding code, tools, moon phase,, etc...

---

_Comment by @kbknapp on 2020-04-29 03:42_

> into every trait except `IntoApp`

This *might* be able to start as just an addition to the `FromArgMatches` trait. I need to re-familiarize myself with the derive code, but off the top of my head it wouldn't need to propagate through the other traits as well.

---

_Comment by @lu-zero on 2020-04-29 09:35_

I started looking at `FromArgMatches`. `gen_constructor` probably could be refactored so I can reuse part of it.

---

_Referenced in [clap-rs/clap#1878](../../clap-rs/clap/pulls/1878.md) on 2020-04-29 09:46_

---

_Comment by @CreepySkeleton on 2020-04-29 12:37_

@kbknapp I'm afraid this is inevitable because we can't "know what's on the other side" in case of flatten , subcommand, etc... I'll explain in the PR thread.

---

_Comment by @lu-zero on 2020-04-29 12:42_

I confirm that subcommand needs an update variant as well.

---

_Closed by @pksunkara on 2020-11-14 11:38_

---

_Referenced in [clap-rs/clap#2605](../../clap-rs/clap/issues/2605.md) on 2021-07-19 16:06_

---
