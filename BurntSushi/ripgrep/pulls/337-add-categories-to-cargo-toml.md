```yaml
number: 337
title: " Add categories to Cargo.toml"
type: pull_request
state: merged
author: shepmaster
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2017-01-20T19:20:36Z
updated_at: 2017-01-21T16:31:51Z
url: https://github.com/BurntSushi/ripgrep/pull/337
synced_at: 2026-01-12T18:23:12Z
```

#  Add categories to Cargo.toml

---

_@shepmaster_


Hi! [crates.io now supports categories][categories], which are a curated list
of topics aimed at helping an end-user coming to crates.io looking for
"a crate to do ______".

We're sending pull requests to selected crates to add categories in order to help 
populate the categories and seed their usefulness. We've made a guess at the best
category/categories for this crate; if it doesn't fit, please feel free to take
a look at [all the available categories and their descriptions][categories] and
[the slug values that should be specified in your Cargo.toml][slugs] and pick
different ones. If you have a category in mind that isn't available, you can
[send a PR to this file on crates.io][categories.toml] to propose additional
categories.

Crates can have up to 5 categories, and uploading categories to crates.io
currently requires publishing a new version with a cargo nightly from 2017-01-18
or later (it needs to contain [this PR][cargo-pr]).

We've [published a blog post][blog-post] with further details about categories.
The blog post also talks about the new crates.io support for CI badges, which
you may be interested in adding as well.

Please let me know if you have any questions!

[categories]: https://crates.io/categories
[slugs]: https://crates.io/category_slugs
[categories.toml]: https://github.com/rust-lang/crates.io/blob/master/src/categories.toml
[cargo-pr]: https://github.com/rust-lang/cargo/pull/3301
[blog-post]: http://integer32.com/2017/01/20/categories-and-ci-badges.html

---

_Comment by @BurntSushi on 2017-01-20 19:26_

Should this also be in the `text-processing` category?

---

_Comment by @shepmaster on 2017-01-20 19:43_

> Should this also be in the text-processing category?

Sure! Are you able to amend my commit? I'm... not 100% sure how to easily amend my commit from the UI 😊 

---

_Merged by @BurntSushi on 2017-01-21 16:31_

---

_Closed by @BurntSushi on 2017-01-21 16:31_

---
