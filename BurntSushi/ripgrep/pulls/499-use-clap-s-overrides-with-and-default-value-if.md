```yaml
number: 499
title: "Use clap's overrides_with and default_value_if"
type: pull_request
state: merged
author: ericbn
labels: []
assignees: []
merged: true
base: master
head: override
created_at: 2017-06-01T23:38:22Z
updated_at: 2017-06-12T14:31:46Z
url: https://github.com/BurntSushi/ripgrep/pull/499
synced_at: 2026-01-12T18:23:13Z
```

# Use clap's overrides_with and default_value_if

---

_@ericbn_

to better organize options. These are the changes:
- color will have default value of "never" if `--vimgrep` is given, and only if no `--color` option is given
- last overrides previous: `--line-number` and `--no-line-number`, `--heading` and `--no-heading`, `--with-filename` and `--no-filename`, and `--vimgrep` and -`-count`
- no heading will be show if `--vimgrep` is defined. This worked inside vim actually because heading is also only shown if tty is stdout (which is not the case when rg is called from vim).

Unfortunately, clap does not behave like a usual GNU/POSIX in some cases, as reported in https://github.com/kbknapp/clap-rs/issues/970 and https://github.com/kbknapp/clap-rs/issues/976 (having all the bells and whistles, on the other hand). So we still have issues like rg failing when same argument is given more than once (unless for the few ones marked with `multiple(true)`), or having unintuitive precedence rules (and probably non-intentional, just there because of clap's limitations) like:
- `--no-filename` over `--vimgrep`
- `--no-line-number` over `--column`, `--pretty` or `--vimgrep`
- `--no-heading` over `--pretty`

regardless of the order in which options where given, where the desired behavior would be that the last option would override the previous ones given.

---

_Comment by @BurntSushi on 2017-06-12 14:29_

Sorry this took me so long to get to, but I think I buy all of this. Thanks for thinking through it!

---

_Merged by @BurntSushi on 2017-06-12 14:29_

---

_Closed by @BurntSushi on 2017-06-12 14:29_

---

_Branch deleted on 2017-06-12 14:31_

---
