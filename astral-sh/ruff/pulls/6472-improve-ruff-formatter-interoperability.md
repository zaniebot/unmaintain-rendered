```yaml
number: 6472
title: Improve Ruff Formatter Interoperability
type: pull_request
state: merged
author: magic-akari
labels:
  - internal
assignees: []
merged: true
base: main
head: fix/fmt
created_at: 2023-08-10T06:53:55Z
updated_at: 2024-01-20T06:40:31Z
url: https://github.com/astral-sh/ruff/pull/6472
synced_at: 2026-01-12T15:55:21Z
```

# Improve Ruff Formatter Interoperability

---

_@magic-akari_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Thank you for developing such an amazing project.

I am currently trying to port the ruff python formatter to wasm. See https://github.com/wasm-fmt/ruff_fmt
I ran into some issues that prevented me from moving forward.

Here are some changes to make it more interoperable for downstream developers:

1. ~Change the visibility of some structs. This is a straightforward task, adding some missing `pub` keywords.~
2. ~Export some default config functions.~
3. Implement `FromStr` for some structs, so that users can easily parse configuration from strings.
4. Fix `with_quote_style`
5. Fix `indent_style` serde default value


## Test Plan

Most of the changes don't involve logic changes, except for a couple of `FromStrs`, and I'll add test cases if necessary.

---

_@MichaReiser reviewed on 2023-08-10 06:55_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/lib.rs`:125 on 2023-08-10 06:55_

Thank you for exploring the use of the formatter in wasm. We already use the formatter in our playground and didn't had a need to make these fields public. Could you explain why it is necessary to make the fields public for your use case. All the fields are intentionally private (here and in `PyFormatOptions`)

Also: We provide no API stability guarantees. Any of this could change at any time.

---

_Review comment by @magic-akari on `crates/ruff_formatter/src/lib.rs`:125 on 2023-08-10 07:12_

I hadn't noticed that there was already a formatter for wasm, I looked at the documentation but didn't find it.
My intention was to export it to the dprint plugin.

---

_@magic-akari reviewed on 2023-08-10 07:12_

---

_@magic-akari reviewed on 2023-08-10 07:15_

---

_Review comment by @magic-akari on `crates/ruff_formatter/src/lib.rs`:125 on 2023-08-10 07:15_

https://github.com/wasm-fmt/ruff_fmt/blob/60669453962a858bbdc3bf015b2f1771603c9104/crates/ruff_fmt_dprint/src/configuration/configuration.rs#L26

https://github.com/wasm-fmt/ruff_fmt/blob/60669453962a858bbdc3bf015b2f1771603c9104/crates/ruff_fmt_dprint/src/configuration/resolve_config.rs#L36-L42

---

_Comment by @magic-akari on 2023-08-10 07:35_

@MichaReiser 
I need to adapt some configuration of dprint.
It looks like ruff's playground formatter is not configurable. 

https://github.com/astral-sh/ruff/blob/ac5c8bb3b683fb009b39098c00f9357e9aeb6e38/crates/ruff_wasm/src/lib.rs#L270-L276

https://github.com/astral-sh/ruff/blob/ac5c8bb3b683fb009b39098c00f9357e9aeb6e38/crates/ruff_python_formatter/src/options.rs#L54-L59

---

_@MichaReiser reviewed on 2023-08-10 07:35_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/lib.rs`:125 on 2023-08-10 07:35_

> [wasm-fmt/ruff_fmt@6066945/crates/ruff_fmt_dprint/src/configuration/configuration.rs#L26](https://github.com/wasm-fmt/ruff_fmt/blob/60669453962a858bbdc3bf015b2f1771603c9104/crates/ruff_fmt_dprint/src/configuration/configuration.rs#L26)

You can use `LineWidth::try_from(u16)` to get a line width

> [wasm-fmt/ruff_fmt@6066945/crates/ruff_fmt_dprint/src/configuration/resolve_config.rs#L36-L42](https://github.com/wasm-fmt/ruff_fmt/blob/60669453962a858bbdc3bf015b2f1771603c9104/crates/ruff_fmt_dprint/src/configuration/resolve_config.rs#L36-L42)

It may be a bit more cumbersome but you can do

```rust
let resolved_config = PyFormatOptions::default()
	.with_indent_style(indent_style)
	.with_line_with(line_with)
	.with_quote_style(quote_style)
	with_magic_trailing_comma(magic_trailing_comma)
```

You may also want to use `PyFormatOptions::from_extension()` or `::from_source_type`

---

_@magic-akari reviewed on 2023-08-10 07:41_

---

_Review comment by @magic-akari on `crates/ruff_formatter/src/lib.rs`:125 on 2023-08-10 07:41_

https://github.com/astral-sh/ruff/blob/ac5c8bb3b683fb009b39098c00f9357e9aeb6e38/crates/ruff_python_formatter/src/options.rs#L69-L72

I suppose the signature of `with_quote_style` should be `mut self` and not a reference type. Is that correct?

---

_@MichaReiser reviewed on 2023-08-10 07:42_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/lib.rs`:125 on 2023-08-10 07:42_

That's correct

---

_Comment by @github-actions[bot] on 2023-08-10 07:50_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.09ms     4.9 MB/sec    1.00      8.2±0.02ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1650.7±37.17µs    10.1 MB/sec    1.03  1699.3±65.30µs     9.8 MB/sec
formatter/numpy/globals.py                 1.00    184.8±4.56µs    16.0 MB/sec    1.03    191.0±8.14µs    15.5 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.08ms     7.3 MB/sec    1.02      3.5±0.08ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     10.2±0.05ms     4.0 MB/sec    1.04     10.6±0.03ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.02      2.8±0.00ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    389.0±0.80µs     7.6 MB/sec    1.00    386.9±0.37µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.04ms     4.8 MB/sec    1.04      5.5±0.02ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.3±0.02ms     7.7 MB/sec    1.08      5.7±0.01ms     7.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1155.4±2.47µs    14.4 MB/sec    1.05   1209.1±9.37µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    134.0±2.83µs    22.0 MB/sec    1.02    136.1±0.38µs    21.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.07ms    10.4 MB/sec    1.04      2.6±0.03ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.05ms     4.1 MB/sec    1.00      9.9±0.06ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1876.0±10.87µs     8.9 MB/sec    1.01  1903.5±18.15µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    190.2±1.33µs    15.5 MB/sec    1.04    198.8±5.75µs    14.8 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.02ms     6.1 MB/sec    1.02      4.3±0.04ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.01     12.5±0.04ms     3.2 MB/sec    1.00     12.5±0.05ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.7±3.62µs     8.2 MB/sec    1.01    365.5±7.61µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.03ms     3.9 MB/sec    1.00      6.5±0.04ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.01      6.8±0.05ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1402.6±6.12µs    11.9 MB/sec    1.00   1407.9±7.61µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    143.6±4.34µs    20.5 MB/sec    1.00    143.4±1.14µs    20.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.04ms     8.3 MB/sec    1.00      3.0±0.02ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @magic-akari on 2023-08-10 08:25_

---

_Comment by @magic-akari on 2023-08-10 08:28_

There are multiple commits here, 
I think it's fine use squash merge or I squash locally and then force push to the branch.

---

_@konstin reviewed on 2023-08-10 09:01_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/options.rs`:41 on 2023-08-10 09:01_

The preferred way to use this is to construct `PyFormatOptions::default()` and then override the fields you need, just like [format_dev](https://github.com/astral-sh/ruff/blob/9bb21283cade3069ff8860477aff120b31492e90/crates/ruff_dev/src/format_dev.rs#L836-L846) does.

---

_@magic-akari reviewed on 2023-08-10 09:28_

---

_Review comment by @magic-akari on `crates/ruff_python_formatter/src/options.rs`:41 on 2023-08-10 09:28_

IndentStyle has its own `Default` value `2`.
LineWidth has its own `Default` value `80`.

Maybe it was from `Rome` config. 
But I think this exported default function is OK for me.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/options.rs`:41 on 2023-08-10 10:27_

What @konstin suggests is that you use `PyFormatOptions::default()` and only set the `indent_style` and `line_with` if you want to override the default value.

---

_@MichaReiser reviewed on 2023-08-10 10:27_

---

_@magic-akari reviewed on 2023-08-10 10:36_

---

_Review comment by @magic-akari on `crates/ruff_python_formatter/src/options.rs`:41 on 2023-08-10 10:36_

This makes sense.

---

_@magic-akari reviewed on 2023-08-10 10:44_

---

_Review comment by @magic-akari on `crates/ruff_python_formatter/src/options.rs`:41 on 2023-08-10 10:44_

@MichaReiser I noticed that when a field is missing, `line_width` will give the same result (both 88).

But `indent_style` is different. 
The default value of `PyFormatOptions` returns `Space(4)`, and serde's default value is `Tab`. 
Is this intentional?

---

_@konstin reviewed on 2023-08-10 11:03_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/options.rs`:41 on 2023-08-10 11:03_

The default value from `IndentStyle` comes from `ruff_formatter`, not from `ruff_python_formatter`. That's why you should start with `PyFormatOptions::default` and get or set everything from that `PyFormatOptions` object, which has the right defaults for python.

---

_@magic-akari reviewed on 2023-08-10 11:38_

---

_Review comment by @magic-akari on `crates/ruff_python_formatter/src/options.rs`:41 on 2023-08-10 11:38_

I mean that the default `indent_style` value should be same in both `PyFormatOptions::default` and serde default .

https://github.com/astral-sh/ruff/blob/ac5c8bb3b683fb009b39098c00f9357e9aeb6e38/crates/ruff_python_formatter/src/options.rs#L16-L23

https://github.com/astral-sh/ruff/blob/ac5c8bb3b683fb009b39098c00f9357e9aeb6e38/crates/ruff_python_formatter/src/options.rs#L36-L46

---

_@MichaReiser approved on 2023-08-10 12:39_

These changes look good to me. Thanks for contributing them upstream. 

I will be interested to see where it goes with your `dprint` implementation. Just a reminder, the formatter API isn't stable, and we may change it in minor releases without notice. 



---

_Merged by @MichaReiser on 2023-08-10 12:39_

---

_Closed by @MichaReiser on 2023-08-10 12:39_

---

_Label `internal` added by @MichaReiser on 2023-08-10 12:40_

---

_Comment by @magic-akari on 2023-08-11 19:29_

You can try it:

1. instal dprint: https://dprint.dev/install/
2. config the plugin
```bash
dprint config add wasm-fmt/ruff_fmt
```

---

I am using ruff formatter as git dependency.
I was wondering if there was a roadmap. I'm waiting for it to be published on crates.

---

_Comment by @MichaReiser on 2023-08-14 07:16_

>  I was wondering if there was a roadmap. I'm waiting for it to be published on crates.

We don't have any near-term plans on publishing our crates to crates.io because people will (not unjustified) assume that the APIs are stable and we aren't ready to give that guarantee yet. 

---

_Branch deleted on 2024-01-20 06:40_

---
