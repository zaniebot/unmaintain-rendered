```yaml
number: 2196
title: Allow templates to change the error message strings
type: issue
state: closed
author: filipelbc
labels:
  - C-enhancement
  - A-help
  - S-duplicate
assignees: []
created_at: 2020-10-31T21:07:14Z
updated_at: 2022-01-11T18:41:11Z
url: https://github.com/clap-rs/clap/issues/2196
synced_at: 2026-01-12T16:14:12Z
```

# Allow templates to change the error message strings

---

_@filipelbc_

Before anything, this library is great! Thanks!

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Describe your use case

I prefer printing `ERROR: ...` instead of `error: ...`, and to make the background red, instead of the foreground.

### Describe the solution you'd like

Allow the user to set the `"error: "` string, which is currently hard-coded, see https://github.com/clap-rs/clap/blob/97b4fb639f846c5b910c318dd6c6f7d6da779d7d/src/parse/errors.rs#L401

Allow the user to change the colors, currently hard-coded, see https://github.com/clap-rs/clap/blob/97b4fb639f846c5b910c318dd6c6f7d6da779d7d/src/output/fmt.rs#L53

You could create a `StyleSetting` struct and a mechanism similar to `AppSettings`.

```rust
struct StyleSetting {
    string: &str,
    style: termcolor::ColorSpec,
}

enum StylePoint {
    ErrorStart,
    UsageStart,
    ...
}

let my_error_style = StyleSetting::new("ERROR: ", my_error_color_spec);

App::new("Foo")
    ...
    .style_setting(ErrorStart, my_error_style)
    ...
```

### Additional context

Similar to https://github.com/clap-rs/clap/issues/2035, but instead of allowing usage of Clap's error formatting, allow the user to customize Clap's messages and style to fit his preferences.

---

_Label `T: new feature` added by @filipelbc on 2020-10-31 21:07_

---

_Renamed from "Allow customization of error message style (and possible others)" to "Allow customization of error message style (and possibly others)" by @filipelbc on 2020-10-31 21:09_

---

_Comment by @pksunkara on 2020-10-31 21:10_

Note: Same as #1433 but this issue needs error message template support.

---

_Added to milestone `3.1` by @pksunkara on 2020-10-31 21:10_

---

_Label `C: colorizing` added by @pksunkara on 2020-10-31 21:11_

---

_Renamed from "Allow customization of error message style (and possibly others)" to "Allow templates to change the error message strings" by @pksunkara on 2020-10-31 21:11_

---

_Comment by @pksunkara on 2020-11-06 20:09_

Might also be related to #1734 

---

_Referenced in [epage/clapng#168](../../epage/clapng/issues/168.md) on 2021-12-06 20:19_

---

_Label `C: colorizing` removed by @epage on 2021-12-08 20:23_

---

_Label `A-help` added by @epage on 2021-12-08 20:23_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:12_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:12_

---

_Comment by @epage on 2021-12-13 15:07_

This request seems most like #1790 which we closed because we are not looking at allowing granular styling of claps API, instead favoring templates and #1433.  Since we have #1734 for an error message template and #1433 for styling it, I'm going to go ahead and close this out.

If there is any concern with this plan, let us know!

---

_Closed by @epage on 2021-12-13 15:07_

---

_Label `S-duplicate` added by @epage on 2022-01-11 18:41_

---
