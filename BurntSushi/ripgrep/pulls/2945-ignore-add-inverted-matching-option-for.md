```yaml
number: 2945
title: "ignore: add `inverted_matching` option for `OverrideBuilder`"
type: pull_request
state: closed
author: yuuhikaze
labels: []
assignees: []
base: master
head: feature/overrides-exclude-functionality
created_at: 2024-12-09T14:53:17Z
updated_at: 2025-08-27T20:11:32Z
url: https://github.com/BurntSushi/ripgrep/pull/2945
synced_at: 2026-01-12T18:23:14Z
```

# ignore: add `inverted_matching` option for `OverrideBuilder`

---

_@yuuhikaze_

The `overrides` module description for the `ignore` crate is as follows:
```markdown
The overrides module provides a way to specify a set of override globs.
This provides functionality similar to `--include` or `--exclude` in command
line tools.
```
However there is no way for the user to configure the `OverrideBuilder` to act as an `--exclude`, precisely this PR implements that.
> Usage of the option can be found [here](https://github.com/yuuhikaze/tern/blob/9932b43905ebf07c499904d0234f6b5c5ec173aa/src/converter.rs#L41).

---

_Comment by @BurntSushi on 2025-08-17 18:04_

Using shared mutable state like this in a library is totally nuts.

And the presumption here isn't correct anyway. The `--exclude` is done by adding a `!` to beginning of the glob. This is even in the public API documentation.

---

_Closed by @BurntSushi on 2025-08-17 18:04_

---

_Comment by @yuuhikaze on 2025-08-27 20:11_

Good afternoon,
Can you explain me why "using shared mutable state like this in a library is totally nuts"?
I still consider the API should provide the functionality proposed in the PR since that would be more idiomatic and transparent rather than telling the user to negate his logic. It is easier to think in a non negative way.
The implementation may not be best, but as the author of the crate, you could come up with something more sophistacated and ad hoc.
Greetings!

---
