```yaml
number: 1943
title: "Placing `--` before a subcommand fails with a confusing error"
type: issue
state: closed
author: Demi-Marie
labels:
  - C-bug
assignees: []
created_at: 2020-05-26T05:58:53Z
updated_at: 2020-10-09T17:21:37Z
url: https://github.com/clap-rs/clap/issues/1943
synced_at: 2026-01-12T16:14:12Z
```

# Placing `--` before a subcommand fails with a confusing error

---

_@Demi-Marie_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

`rustc 1.43.1 (8d69840ab 2020-05-04)`

### Affected Version of clap

2.33.0

### Expected Behavior Summary

If `x` is a subcommand, then `./some_binary -- x` should give a message stating that `--` before a subcommand name is not allowed.

### Actual Behavior Summary

`./some_binary -- x` suggests that I re-run `./some_binary -- x`.

### Steps to Reproduce the issue

Include `--` before a subcommand name

### Sample Code or Link to Sample Code

[This commit of Ledgeracio](https://github.com/paritytech/ledgeracio/tree/39f34590b4ec27c8f8cb2f004931e5f35bf50cbf) reproduces the problem when `cargo run -- -- stash` is run.

### Debug output

Compile clap with `debug` feature:

```toml
[dependencies]
clap = { version = "*", features = ["debug"] }
```

The output may be very long, so feel free to link to a gist or attach a text file

<details>
<summary> Debug Output </summary>
<pre>
<code>
DEBUG:clap:Parser::add_subcommand: term_w=None, name=show
DEBUG:clap:Parser::add_subcommand: term_w=None, name=status
DEBUG:clap:Parser::add_subcommand: term_w=None, name=claim
DEBUG:clap:Parser::add_subcommand: term_w=None, name=submit-validator-set
DEBUG:clap:Parser::add_subcommand: term_w=None, name=add-controller-key
DEBUG:clap:Parser::add_subcommand: term_w=None, name=stash
DEBUG:clap:Parser::add_subcommand: term_w=None, name=status
DEBUG:clap:Parser::add_subcommand: term_w=None, name=announce
DEBUG:clap:Parser::add_subcommand: term_w=None, name=replace-key
DEBUG:clap:Parser::add_subcommand: term_w=None, name=generate-keys
DEBUG:clap:Parser::add_subcommand: term_w=None, name=validator
DEBUG:clap:Parser::propagate_settings: self=Ledgeracio, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=stash, settings=AppFlags(
    SC_REQUIRED_ELSE_HELP | NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=stash, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=show, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=show, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=status, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=status, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=claim, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=claim, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=submit-validator-set, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=submit-validator-set, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=add-controller-key, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=add-controller-key, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=validator, settings=AppFlags(
    SC_REQUIRED_ELSE_HELP | NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=validator, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=status, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=status, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=announce, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=announce, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=replace-key, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=replace-key, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=generate-keys, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=generate-keys, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::create_help_and_version: Building help
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--"' ([45, 45])
DEBUG:clap:Parser::is_new_arg:"--":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: -- found
DEBUG:clap:Parser::get_matches_with: setting TrailingVals=true
DEBUG:clap:Parser::get_matches_with: Begin parsing '"stash"' ([115, 116, 97, 115, 104])
DEBUG:clap:Parser::is_new_arg:"stash":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=dry-run;
DEBUG:clap:Parser::groups_for_arg: name=dry-run
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:usage::needs_flags_tag:iter: [FLAGS] required
DEBUG:clap:usage::create_help_usage: usage=ledgeracio [FLAGS] [OPTIONS] &lt;SUBCOMMAND&gt;
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Colorizer::error;
DEBUG:clap:Colorizer::warning;
DEBUG:clap:Colorizer::good;
DEBUG:clap:Colorizer::good;
DEBUG:clap:Colorizer::good;

</code>
</pre>
</details>


---

_Added to milestone `3.1` by @pksunkara on 2020-05-26 06:36_

---

_Label `C: usage strings` added by @pksunkara on 2020-05-26 06:36_

---

_Label `T: bug` added by @pksunkara on 2020-05-26 06:36_

---

_Comment by @kbknapp on 2020-05-26 22:04_

Upgrading to 3.0-beta.1 provides a better, but still not perfect message:

```
cargo run -- -- stash
   Compiling ledgeracio v0.1.0 (/home/kevin/.tmp/ledgeracio)
    Finished dev [unoptimized + debuginfo] target(s) in 5.98s
     Running `target/debug/ledgeracio -- stash`
error: Found argument 'stash' which wasn't expected, or isn't valid in this context

If you tried to supply `stash` as a PATTERN use `-- stash`

USAGE:
    ledgeracio [FLAGS] [OPTIONS] <SUBCOMMAND>

For more information try --help
```

---

_Comment by @kbknapp on 2020-05-26 22:06_

Basically, we shouldn't include this line:

```
If you tried to supply `stash` as a PATTERN use `-- stash`
```

if `--` was used.

---

_Label `C: usage strings` removed by @kbknapp on 2020-05-26 22:07_

---

_Label `C: errors` added by @kbknapp on 2020-05-26 22:07_

---

_Comment by @CreepySkeleton on 2020-05-27 00:07_

And check the first arg after the `--`. If it's a subcommand, show a friendly hint.

---

_Comment by @ldm0 on 2020-10-06 01:09_

Related: <https://github.com/clap-rs/clap/issues/1744>

---

_Referenced in [clap-rs/clap#2154](../../clap-rs/clap/pulls/2154.md) on 2020-10-06 12:08_

---

_Referenced in [clap-rs/clap#2157](../../clap-rs/clap/pulls/2157.md) on 2020-10-08 02:42_

---

_Closed by @bors[bot] on 2020-10-09 17:21_

---
