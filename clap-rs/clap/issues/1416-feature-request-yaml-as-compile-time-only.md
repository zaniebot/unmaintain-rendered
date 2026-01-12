```yaml
number: 1416
title: "Feature request: YAML as compile-time only dependency"
type: issue
state: closed
author: devinrsmith
labels:
  - E-hard
  - E-help-wanted
assignees: []
created_at: 2019-02-17T14:54:27Z
updated_at: 2021-12-08T20:00:00Z
url: https://github.com/clap-rs/clap/issues/1416
synced_at: 2026-01-12T16:14:10Z
```

# Feature request: YAML as compile-time only dependency

---

_@devinrsmith_

I think that the YAML dependency could be a compile-time only dependency. That would be the best of both world IMO - keep the source neat and tidy, no need for runtime parsing of the parser, and no runtime YAML dependency!

[build-scripts](https://doc.rust-lang.org/cargo/reference/build-scripts.html)
[code generation / build-dependencies](https://rust-lang-nursery.github.io/rust-cookbook/development_tools/build_tools.html)

---

_Comment by @Kage-Yami on 2020-01-04 12:48_

I was also thinking along these lines, but after reading [Specifying Dependencies -> Build dependencies](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#build-dependencies), don't they only apply to `build.rs` (and in fact specifically _don't_ apply to the app itself)? If so, then I don't see how this would work, as I would think the CLI definition needs to happen _in the app_, rather than a build script...

---

_Comment by @CreepySkeleton on 2020-01-05 05:10_

@Kage-Yami is right here, `dev-depandencies` are exposed only to the build script, the app has no access to them. This is not doable, at least not with current API.

---

_Label `not-sure` added by @CreepySkeleton on 2020-02-01 15:09_

---

_Comment by @CreepySkeleton on 2020-02-06 09:17_

We can actually make a proc-macro that parses `.yaml` and expands to resulting `clap::App`, no runtime cost involved!

---

_Label `cc: CreepySkeleton` removed by @CreepySkeleton on 2020-02-06 09:19_

---

_Label `C: macros` added by @CreepySkeleton on 2020-02-06 09:19_

---

_Label `C: yaml parser` added by @CreepySkeleton on 2020-02-06 09:19_

---

_Label `D: hard` added by @CreepySkeleton on 2020-02-06 09:19_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-06 09:19_

---

_Label `T: new feature` added by @CreepySkeleton on 2020-02-06 09:19_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-06 09:19_

---

_Added to milestone `3.1` by @pksunkara on 2020-02-06 09:27_

---

_Comment by @pksunkara on 2020-02-13 07:40_

The main concern with a proc-macro that parser `.yaml` is custom spans. For example, let's assume we have a proc-macro `load_yaml` that builds an App out of yml file. And if the yml file has an error, we should emit a `proc-macro-error` pointing to the issue which is difficult without us being able to build custom spans (since we won't be tokenizing yml file directly but instead reading the file from fs).

Can we use serde to change something about how we can load the yml during compile time?

**NOTE**: I looked at the related code, and it looks like current yaml **is** a run time dependency because it's loading yaml from the `yml` static string (expanded by `include_str!`).

---

_Comment by @CreepySkeleton on 2020-02-13 08:39_

We can't exactly *point* there indeed, just because files included via `include*!` or non-rs files don't really have span info attached to them. What we can do is to print line/column the error was encountered at, and I think it should be "good enough". `serde` would grant us this info out of box.

---

_Comment by @pksunkara on 2020-02-13 09:04_

Yeah, but can we create a span with the line, column info? That would make the error reporting easy

---

_Comment by @CreepySkeleton on 2020-02-13 09:20_

We can't _create_ a span with a custom line/column info since the only ways are `call_site`, `def_site`, `mixed_site`. We can't _set_ this info on already existing span because there's no such API, even unstable. 

Could you explain what you expect from such API? The error cannot be rendered for files that are not part of a crate.

---

_Comment by @pksunkara on 2020-02-13 20:52_

> We can't set this info on already existing span because there's no such API, even unstable.

I wanted to make up Span. For files that can not be token streams, but we still want to parse, I wanted to make up Span when I get error and let `compile_error!` or `Diagnostic` API take care of printing it for me.

So, I looked into it and proc_macro2 stores the source map locally and thus we can't fool it by making up `Span`.

---

_Comment by @pickfire on 2020-02-29 01:13_

Is `const` able to help to compile this during compile time? https://github.com/rust-lang/rfcs/blob/master/text/0246-const-vs-static.md

---

_Comment by @CreepySkeleton on 2020-02-29 04:40_

@pickfire Nope, since yaml loader is not `const` at all. We need a proper procedural macro for that.

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [epage/clapng#113](../../epage/clapng/issues/113.md) on 2021-12-06 18:34_

---

_Comment by @epage on 2021-12-08 20:00_

See https://github.com/clap-rs/clap/issues/3087.  I expect any effort in this direction would be in an independent crate.

---

_Closed by @epage on 2021-12-08 20:00_

---
