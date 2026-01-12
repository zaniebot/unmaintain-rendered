```yaml
number: 1329
title: Generate JSON schema for Ruff options
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: toml-json-schema
created_at: 2022-12-22T06:29:52Z
updated_at: 2022-12-24T23:33:45Z
url: https://github.com/astral-sh/ruff/pull/1329
synced_at: 2026-01-12T05:36:31Z
```

# Generate JSON schema for Ruff options

---

_Pull request opened by @edgarrmondragon on 2022-12-22 06:29_

See https://github.com/charliermarsh/ruff/issues/1217

---

_Converted to draft by @edgarrmondragon on 2022-12-22 06:30_

---

_@squiddy reviewed on 2022-12-22 06:54_

---

_Review comment by @squiddy on `src/settings/options.rs`:260 on 2022-12-22 06:54_

I think it's unfortunate if we have to repeat ourselves here. I would look into whether we can adjust the `option` macro to attach the `doc` attribute to the field (would probably be nice during development as well), or whether we can make use of the generated `ConfigurationOptions` struct that exposes all that information.

---

_Review comment by @squiddy on `src/pyupgrade/settings.rs`:11 on 2022-12-22 06:55_

According to the docs, this would also be an option.

```rust
#[serde(deny_unknown_fields, rename_all = "kebab-case", rename = "PyUpgradeOptions")]
pub struct Options {
```

---

_@squiddy reviewed on 2022-12-22 06:55_

---

_@edgarrmondragon reviewed on 2022-12-22 07:30_

---

_Review comment by @edgarrmondragon on `src/settings/options.rs`:260 on 2022-12-22 07:30_

I agree. I'll look into it

---

_Comment by @squiddy on 2022-12-22 10:32_

This is nice. I'm keeping an eye on this, because having a schema could come in handy for the wasm playground, where I currently export some custom schema to have this options sidebar. If there was a way for this schema to also have the default values in there, I could use just this. :) 

---

_Review comment by @squiddy on `src/settings/options.rs`:260 on 2022-12-22 10:35_

We can also explore turning this around: see how far we can get with JSON schema & schemars, and perhaps we can build on that as a base for the [option] macros.

---

_@squiddy reviewed on 2022-12-22 10:35_

---

_@charliermarsh reviewed on 2022-12-22 15:58_

---

_Review comment by @charliermarsh on `src/settings/options.rs`:260 on 2022-12-22 15:58_

Repeating ourselves here isn't the end of the world (it's nice to have these as rust docs too, I guess?) though agree it'd be nice to reuse if there's a straightforward path.

---

_Comment by @charliermarsh on 2022-12-22 15:58_

This is awesome.

---

_@edgarrmondragon reviewed on 2022-12-23 04:07_

---

_Review comment by @edgarrmondragon on `src/settings/options.rs`:260 on 2022-12-23 04:07_

Yeah, there's a bit of overlap between the two traits so at some point (not necessarily in this PR) a single source of truth should be in place.

---

_Marked ready for review by @edgarrmondragon on 2022-12-23 04:07_

---

_@charliermarsh reviewed on 2022-12-23 05:07_

---

_Review comment by @charliermarsh on `ruff.schema.json`:6 on 2022-12-23 05:07_

Are these missing descriptions?

---

_Review comment by @charliermarsh on `src/settings/options.rs`:260 on 2022-12-24 04:59_

Okay, I think I have this working in the other direction?

Like we can do this:

```rust
#[option(
    default = r#"[]"#,
    value_type = "Vec<char>",
    example = r#"
        # Allow minus-sign (U+2212), greek-small-letter-rho (U+03C1), and the asterisk-operator (U+2217),
        # which could be confused for "-", "p", and "*", respectively.
        allowed-confusables = ["−", "ρ", "∗"]
    "#
)]
/// A list of allowed "confusable" Unicode characters to ignore when enforcing `RUF001`,
/// `RUF002`, and `RUF003`.
pub allowed_confusables: Option<Vec<char>>,
...
```

...and have it generate the schema, and the documentation, and it works in the IDE.

What do you think @squiddy?

---

_@charliermarsh reviewed on 2022-12-24 04:59_

---

_@charliermarsh reviewed on 2022-12-24 05:00_

---

_Review comment by @charliermarsh on `src/settings/options.rs`:260 on 2022-12-24 05:00_

(It's probably possible to generate the rustdoc from the proc macro, but I'm not that strong on macros (and I don't know how that would ever work with the IDE).)

---

_@squiddy reviewed on 2022-12-24 05:30_

---

_Review comment by @squiddy on `src/settings/options.rs`:260 on 2022-12-24 05:30_

Yes, looks good to me! As you said, that fit's more into standard rust and how IDEs work. I found some hints on how to generate rustdoc from the macro, but than I ran into macro errors that I couldn't resolve.. probably for the best.

(On that topic, I wonder if we can just move everything into rustdoc and have the macro parse that text up into `default` / `value_type` and `example`.)

---

_@edgarrmondragon reviewed on 2022-12-24 05:51_

---

_Review comment by @edgarrmondragon on `ruff.schema.json`:6 on 2022-12-24 05:51_

Yes, there are a bunch of missing descriptions. I can add them :)

---

_@charliermarsh reviewed on 2022-12-24 05:56_

---

_Review comment by @charliermarsh on `ruff.schema.json`:6 on 2022-12-24 05:56_

It’s ok, I think I can update the macro to read from rustdoc, and then these comments can just be taking the “doc” piece from above and moving them into comments verbatim.

---

_@charliermarsh reviewed on 2022-12-24 05:58_

---

_Review comment by @charliermarsh on `ruff.schema.json`:6 on 2022-12-24 05:58_

The most helpful thing, in that light, would just be to take the existing doc= text from above and copying it as // docs for each attribute. But I can also do it, either way.

---

_Merged by @charliermarsh on 2022-12-24 19:10_

---

_Closed by @charliermarsh on 2022-12-24 19:10_

---

_Comment by @charliermarsh on 2022-12-24 19:10_

Thanks @edgarrmondragon as always!

---

_Branch deleted on 2022-12-24 23:33_

---
