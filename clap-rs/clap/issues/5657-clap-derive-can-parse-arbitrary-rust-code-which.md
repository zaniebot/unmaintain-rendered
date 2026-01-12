```yaml
number: 5657
title: clap_derive can parse arbitrary Rust code, which is not always necessary and costs compile time
type: issue
state: open
author: hanna-kruppe
labels:
  - C-enhancement
  - M-breaking-change
  - S-waiting-on-decision
  - A-derive
assignees: []
created_at: 2024-08-10T12:58:08Z
updated_at: 2024-11-04T16:37:33Z
url: https://github.com/clap-rs/clap/issues/5657
synced_at: 2026-01-12T16:14:17Z
```

# clap_derive can parse arbitrary Rust code, which is not always necessary and costs compile time

---

_@hanna-kruppe_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.15

### Describe your use case

I have a workspace where I care a little too much about build times (e.g., for iterating on cargo profile settings / RUSTFLAGS) and use a handful of syn-based custom derives, including clap's. Unlike most other derives I've seen (and unlike all others I'm using in this workspace), clap_derive enables syn's "full" feature that enables parsing arbitrary expressions, items, etc. instead of just the parts that are usually needed for derives. This has negative effects for compilation time (cc #2037):

1. It makes syn take longer to compile (by ca. one second on my machine), which due to feature unification holds back *everything* that depends on syn. In my specific case, this only has negligible impact (ca. 0.2s wall clock time for clean `cargo test`, out of ca. 10s total) because there happens to be enough independent work for the number of CPUs I have and another critical path in the schedule. It matters more in smaller workspaces (or with more CPUs, presumably), e.g., for the git-derive example by itself, disabling syn/full reduces clean dev builds from 5s to 4.3s on my machine.
2. When different workspace members want different sets of features from syn, this means syn and everything it depends on may get rebuilt multiple times while switching between workspace members (i.e., unlike the previous point this also affects some incremental builds). This is the [problem that the workspace-hack pattern addresses](https://docs.rs/cargo-hakari/latest/cargo_hakari/about/index.html). But even with the wonderful cargo-hakari, it takes some manual effort to set up and maintain a workspace hack, so many projects don't do that. (I currently do it in the workspace in question, but it provides less value nowadays than when I first set it up, so I've considered dropping it.)

### Describe the solution you'd like

Ideally, the syn/full feature could just be removed completely (cc #4784). clap_derive does build when the feature is removed (some care is needed while testing this because several dev-dependencies in clap's workspace enable it anyway), but some *uses* of the derives break because syn can't parse some kinds of expressions without the "full" feature. I don't have a full list of what's affected, but at least it causes an error for `arg(num_args = n..=m)` as used e.g., in the git-derive example. From my understanding of how clap_derive works, I *think* it's always possible to work around this by extracting those expressions into separate constants, but that's still annoying and a breaking change.

On the other hand, many simpler expressions (literals, identifiers, paths, binary operators, taking references, etc.) don't seem to be affected. Case in point, my aforementioned workspace works just fine if test a patched version of clap_derive minus syn/full via `[patch.crates-io]`. So instead I propose a new feature, enabled by default, flag that controls whether syn/full is enabled. Those who care to fiddle around with feature flags and find that disabling this one doesn't break their project can do so, while everyone else is unaffected.

### Alternatives, if applicable

In addition to or instead of a new feature flag, the breaking change of not enabling syn/full (by default or ever) could be rolled into clap v5. But I don't seriously want to propose this without fully understanding the consequences. Are there any clap features that would become annoying or even impossible to use via the derive API without syn/full? For example, is there an ergonomic alternative for `num_args = n..=m`?

### Additional Context

_No response_

---

_Label `C-enhancement` added by @hanna-kruppe on 2024-08-10 12:58_

---

_Label `M-breaking-change` added by @epage on 2024-08-12 17:35_

---

_Label `A-derive` added by @epage on 2024-08-12 17:35_

---

_Label `S-waiting-on-decision` added by @epage on 2024-08-12 17:35_

---

_Comment by @epage on 2024-08-12 17:38_

Note that anything we do here would be a breaking change.

For myself, I feel like feature flags are a crude tool to work with and try as much as possible to reduce their use.  From what you've described, I'm not sure there is enough benefit for this to outweigh that cost.

If we do anything with this, know of any good documentation on the difference between `parsing` and `full` with `syn?  The docs don't make it too clear what all is lost or what workarounds are available.  I'm loath to use the `serde` approach of having attribute values be strings.

---

_Comment by @hanna-kruppe on 2024-08-12 18:51_

I haven't found much documentation on the difference when I looked into this, but there are clues. The doc comment for syn::Expr says "most of the variants are not available unless “full” is enabled" and each variant just wraps an `ExprWhatever` struct that is annotated (on docs.rs) with what features it requires. Grouped by features and stripped of the common Expr prefix:

- Requires "full": Array, Assign, Async, Await, Block, Break, Closure, Const, Continue, ForLoop, Group, If, Infer, Let, Loop, Match, Range, Repeat, Return, Try, TryBlock, Tuple, Unsafe, While, Yield
- Requires "full" or "derive": Binary, Call, Cast, Field, Index, Lit, Macro, MethodCall, Paren, Path, Reference, Struct, Unary

Looking at this list, ExprRange and possibly ExprArray (`[a, b, c]`) seem to be the only "full"-exclusive ones that are likely to be commonly useful for clap (but I don't know the full API surface area by heart). On the other hand I'm surprised how many things are already enabled by "derive" alone (struct literals, method calls, macros, etc.). It seems that  quite a few expressions kinds were moved out of full and into derive in the past year (e.g., struct literals in https://github.com/dtolnay/syn/commit/3b947f368a1d946b05bb36d10645fd41105e3718). I wonder why the line was drawn exactly there, and why this has changed over time. If syn can already parse that many different expressions without "full", perhaps the missing expression variants that clap needs could also be added with little cost?

---

_Comment by @epage on 2024-08-12 19:07_

`Tuple` is another likely case.

`Closure` is questionable.  It can be convenient but anything larger than a single value should likely be a separate function.

If someone is willing to see if `syn` would move these to `derive`, I would be open to exploring this change during 5.0.

---

_Comment by @hanna-kruppe on 2024-11-03 15:36_

While preparing to open a syn issue, I found out it already has some support for handling these and other expressions without "full", if only to figure out the extent of the expression and package those parts of the token stream into the `Expr::Verbatim` variant. For example, this enum definition can be parsed as `DeriveInput` without "full":

```rust
enum Foo {
    A = [1, 2, 3].len(),
    B = range_length(4..9),
    C = (|x| x * 2)(6),
}
```

This was added specifically for discriminant expressions (https://github.com/dtolnay/syn/issues/1513). As far as I can tell, it *currently* can't be used for anything else, like clap_derive's attribute parsing, but that doesn't seem like a fundamental limitation. Exposing this ability may be an easier ask (fewer downsides e.g. for compile time) than moving more code from "full" into "derive". The behavioral differences would be:

- There's no AST to inspect, it's just a flat `TokenStream`. From what I've seen clap_derive doesn't need that, it just stores the `syn::Expr`s and eventually converts them back into tokens for the expansion.
- Presumably this "expression scanner" is a bit more lenient than the proper expression parser, i.e., it may accept more and possibly invalid syntax. But when the tokens get passed through unmodified, you'd hopefully still get a legible error from rustc with the proper spans and everything. Plus, it may also accept some new *valid* syntax that syn's parser has not been updated to handle yet.

So now my plan changed to asking syn to expose this "recognize expression, stuff tokens in `Expr::Verbatim`" code in a way clap_derive can use. Before I open a syn issue for that, question to @epage: does that sound good from your side?

---

_Comment by @epage on 2024-11-04 16:37_

I would be open to using that!

---

_Referenced in [dtolnay/syn#1782](../../dtolnay/syn/issues/1782.md) on 2024-11-17 15:52_

---
