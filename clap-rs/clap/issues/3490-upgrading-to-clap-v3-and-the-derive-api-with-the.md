```yaml
number: 3490
title: Upgrading to clap v3 and the derive API with the current docs is painful
type: issue
state: closed
author: edmorley
labels: []
assignees: []
created_at: 2022-02-19T16:01:28Z
updated_at: 2022-02-20T00:55:32Z
url: https://github.com/clap-rs/clap/issues/3490
synced_at: 2026-01-12T16:14:15Z
```

# Upgrading to clap v3 and the derive API with the current docs is painful

---

_@edmorley_

Hi! Thank you for all your work on Clap :-)

I'm in the process of converting a Clap v2 app to Clap v3.1.0.

I decided to use the new derive API, since https://docs.rs/clap/latest/clap/#selecting-an-api describes it as:

> Easier to read, write, and modify

As the first part of migrating, I wanted to configure the app settings using the derive API.

Our existing clap v2 implementation does:

```rust
pub(crate) fn setup_cli_parsing<'a, 'b>() -> clap::App<'a, 'b> {
    App::new(env!("CARGO_PKG_NAME"))
        // ...
        .setting(AppSettings::GlobalVersion)
        // ...
}
```

Following the derive tutorial, I found an example for app settings, in `02_app_settings.rs`:
https://github.com/clap-rs/clap/blob/f7b5cdc10809fd93fd0ce4639be926e5063ef8a1/examples/tutorial_derive/02_app_settings.rs#L7-L8

As such, in order to set `AppSettings::GlobalVersion` I tried using:

```rust
#[derive(Parser)]
#[clap(version, about, long_about = None)]
#[clap(global_setting(AppSettings::GlobalVersion))]
pub(crate) enum Cli {
    // ...
}
```

However this gave a deprecation warning:

```
warning: use of deprecated variant `clap::AppSettings::GlobalVersion`: Replaced with `AppSettings::PropagateVersion`
  --> libcnb-cargo/src/cli.rs:42:36
   |
42 | #[clap(global_setting(AppSettings::GlobalVersion))]
   |                                    ^^^^^^^^^^^^^
   |
```

Trying that suggestion (ie: `s/GlobalVersion/PropagateVersion/` in the snippet above) gave yet another deprecation warning:

```
warning: use of deprecated variant `clap::AppSettings::PropagateVersion`: Replaced with `Command::propagate_version` and `Command::is_propagate_version_set`
  --> libcnb-cargo/src/cli.rs:41:36
   |
41 | #[clap(global_setting(AppSettings::PropagateVersion))]
   |                                    ^^^^^^^^^^^^^^^^
   |
```

Trying that suggestion, resulted in code that doesn't compile:

```rust
use clap::{Command, Parser};

#[derive(Parser)]
#[clap(version, about, long_about = None)]
#[clap(global_setting(Command::propagate_version))]
pub(crate) enum Cli {
    // ...
}
```

```
error[E0308]: mismatched types
  --> libcnb-cargo/src/cli.rs:41:23
   |
41 | #[clap(global_setting(Command::propagate_version))]
   |                       ^^^^^^^^^^^^^^^^^^^^^^^^^^ expected enum `AppSettings`, found fn item
   |
   = note: expected enum `AppSettings`
           found fn item `fn(App<'_>, bool) -> App<'_> {App::<'_>::propagate_version}`
```

After that, I tried looking on https://docs.rs/clap/latest/clap/ to see if there was a reference about how to map builder API names to derive API names, but the only useful content on there I could find was links back to the tutorial.

Revisiting the tutorial, I noticed this (which I'd missed first time around, since it's pretty hard to spot):

> In addition to this tutorial, see the [derive reference](https://github.com/clap-rs/clap/blob/master/examples/derive_ref/README.md).

However looking at that [derive reference](https://github.com/clap-rs/clap/blob/master/examples/derive_ref/README.md) page, there are zero references to (or examples of) `global_setting`, and no mention of how to use `Command::*` settings.

After that, I tried searching the repo for `global_setting` to see if there were additional examples (eg https://github.com/clap-rs/clap/search?q=global_setting), however that only turned up usages of the deprecated syntax, eg:
https://github.com/clap-rs/clap/blob/416ac7155a383f90273ebc60b9f1866ee779bce6/tests/derive/structopt.rs#L9

Eventually, searching for `propagate_version` I found this usage in a later tutorial step, which seems to be what I need:
https://github.com/clap-rs/clap/blob/7aa45667f5fb364dec4fe0ea9303164ed4d9e171/examples/tutorial_derive/03_04_subcommands.rs#L5

Since the tutorial only links to examples, rather than inlining them in the markdown, I hadn't seen that, since I hadn't gotten as subcommands yet.

---

Anyway, sorry for the long form issue - I just thought it would be useful to describe the discovery steps I went through - since the process of using Clap v3 with the derive API is really very painful/frustrating at the moment - and it's hard to capture the intersection of several rough edges via individual bug reports.

I'm just left feeling a bit puzzled that the project docs landing page seems to be encouraging people to use the derive API, when most of the docs, deprecation messages, changelog entries etc seem to only reference the builder API, with the derive API seemingly a second class citizen, that's much harder for Clap beginners to use?

Things that may help:
1. Ensure that deprecation messages when using the derive API give suggested changes that work with the derive API, not just the builder API.
2. Make the [derive reference](https://github.com/clap-rs/clap/blob/master/examples/derive_ref/README.md) page easier to discover, by linking to it directly from more places. For example link to it from https://docs.rs/clap/latest/clap/#selecting-an-api, since whilst I've since discovered it's linked from the very top of the docs.rs page -- that section at the top looks like a table of contents (ie: that duplicates the headings lower down), whereas it's actually not. In addition, the mention of the derive reference on the derive tutorial page could be made more obvious.
3. Add missing content to [derive reference](https://github.com/clap-rs/clap/blob/master/examples/derive_ref/README.md) - for example talk about `global_setting` and `setting` etc.
4. Consider making the tutorials focus more on the code examples, and less on lots of duplicated console output. Having to click through to each code example, rather than it being inline in the markdown really breaks the flow of trying to learn and makes the content less discoverable (too easy to skip over a section and not think it's relevant).
5. Update the deprecation messages for `AppSettings::GlobalVersion` (and any others like it) so they don't suggest using an API which itself is also deprecated.
6. In changelog entries, mention both the derive forms and builder API forms, not just the latter. (Particularly for deprecations)

Many thanks!

---

_Comment by @jaskij on 2022-02-19 23:15_

As a workaround until the docs are up to speed, the derive API is largely based on [`structopt`](https://docs.rs/structopt/latest/structopt/), reading their documentation should be helpful.

In fact, structopt's docs have this to say:

> As clap v3 is now out, and the structopt features are integrated into (almost as-is), structopt is now in maintenance mode: no new feature will be added.

@epage in fact, adding a short note linking to structopt might be a decent stopgap.

---

_Comment by @epage on 2022-02-20 00:53_

> I decided to use the new derive API, since https://docs.rs/clap/latest/clap/#selecting-an-api describes it as:

It looks like you were trying to both migrate from clap 2 to clap 3 and from builder to derive at the same time.  I would recommend doing these one at a time.  I might even recommend clap 2 to clap 3.0 to clap 3.1 to derive.  Having too many variables at play when porting between things makes it harder to follow and harder to root cause failures.

I think this would have also made it easier to understand what the deprecation messages was intending.  `App` was renamed to `Command` and we have deprecated all `setting`s, replacing them with functions on `Command`.

> After that, I tried looking on https://docs.rs/clap/latest/clap/ to see if there was a reference about how to map builder API names to derive API names, but the only useful content on there I could find was links back to the tutorial.
>
> Revisiting the tutorial, I noticed this

The derive reference is linked to directly from docs.rs/clap and our README.  Any input on why that was missed so we can explore how to make that more discoverable?

> (which I'd missed first time around, since it's pretty hard to spot):

This is in the introductory section of the tutorial.  Any thoughts on why you missed this and what we can do to improve the discoverability?

> Ensure that deprecation messages when using the derive API give suggested changes that work with the derive API, not just the builder API.

There is only one set of deprecation messages, so we have to give them from the perspective of the builder API.

> For example link to it from https://docs.rs/clap/latest/clap/#selecting-an-api, 

The tutorial is a more appropriate starting off point when someone is selecting an API.  And we link from the tutorial, docs.rs/clap, and the README to the derive, so I think I'd rather focus on simpler messaging here rather than linking to all related documentation,

> Add missing content to [derive reference](https://github.com/clap-rs/clap/blob/master/examples/derive_ref/README.md) - for example talk about global_setting and setting etc.

As I said earlier, those are going away.  Overall in the derive reference, we are focusing on the principles for how the API works (how to map between derive and builder) rather than trying to document how each item in the builder API gets exposed in the derive API.

> Consider making the tutorials focus more on the code examples, and less on lots of duplicated console output. Having to click through to each code example, rather than it being inline in the markdown really breaks the flow of trying to learn and makes the content less discoverable (too easy to skip over a section and not think it's relevant).

In the past, our examples were just code samples with inline documentation.  Our examples were broken for a long time.  I know the current system isn't perfect but this is a way we can have tests for our examples to keep them up to date.  I also think seeing real-world output helps make it clear what the code is doing.

One option we have considered for this is if we switched to `mdBook` we can test the examples like we do today and have the markdown also source content from the `.rs` files so it is all in one place.  One of the challenges is we are torn between user requests that want to push us towards embedding all documentation in empty modules for docs.rs and others who want a mdBook site.

> Update the deprecation messages for AppSettings::GlobalVersion (and any others like it) so they don't suggest using an API which itself is also deprecated.

I get this. On the flip side is the maintenance and verification involved in updating past messages.

---

_Comment by @epage on 2022-02-20 00:54_

> @epage in fact, adding a short note linking to structopt might be a decent stopgap.

Everything covered in the structopt documentation is covered in clap documentation.  I'd rather focus on improving that than punting to incompatible documentation that will just lead to more confusion

---

_Comment by @epage on 2022-02-20 00:55_

With all of that said, we do have [an open issue](https://github.com/clap-rs/clap/issues/3189) for the derive documentation.  I appreciate the user report and wanted to directly respond to your feedback but I would prefer if we move any further conversation over to that.

---

_Closed by @epage on 2022-02-20 00:55_

---
