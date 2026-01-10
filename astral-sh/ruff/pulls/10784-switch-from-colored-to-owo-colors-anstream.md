```yaml
number: 10784
title: Switch from colored to owo_colors/anstream
type: pull_request
state: closed
author: carljm
labels:
  - internal
assignees: []
base: main
head: cjm/colors
created_at: 2024-04-05T00:10:57Z
updated_at: 2024-06-04T16:01:07Z
url: https://github.com/astral-sh/ruff/pull/10784
synced_at: 2026-01-10T21:55:59Z
```

# Switch from colored to owo_colors/anstream

---

_Pull request opened by @carljm on 2024-04-05 00:10_

Refs #5499

## Summary

This switches us from the `colored` crate to the `owo-colors` and `anstream` crates for handling terminal colors. The eventual goal here is to support the `FORCE_COLOR` env var, but this PR doesn't actually do that yet; that'll come in a separate PR.

The approach here is to separate concerns: we always write colored text (using `owo-colors`) and we wrap our output streams in `anstream::AutoStream`, which can detect if the terminal supports colors, and strip out the ANSI color codes if not.

Some points for reviewer attention:

1. I stripped out the fix for ANSI color codes in Windows 10 that was added in #3583, because it's not clear if this will still be needed on Windows, or was just working around a bug in the `colored` crate. It's also not clear what the equivalent of this workaround for `anstream` would be. We need someone to test this on Windows 10, which I don't have available; if nobody else does either, I can get a VM.
2. Some test snapshot outputs changed where they include invalid / non-printable characters, because `anstream` is doing something odd with them when it detects a non-terminal. This won't change things for most users who are running from a terminal. Need to look into this more to understand what's happening; I'd initially thought anstream was stripping out the invalid characters, but in the github diff view it doesn't look like that's quite what's happening. Not sure what we can do differently here, short of scrapping the whole approach of using `anstream`.
3. There's one place I had to replace `let tip = "...".bold().green();` with `let tip = "...".bold(); let tip = tip.green();`. This is because with `owo-colors` the former causes a compiler error: `"...".bold()` creates a temporary that is freed before `tip` is used. Naming the temporary allows it to live long enough. This was only a problem one place, since most places we use chained styling methods, the result is used immediately, so there isn't a lifetime problem. Naming the temporary was the compiler-suggested fix; if there's a nicer way let me know!
4. The new `ruff_linter::colors;` module isn't strictly needed in this PR (at the moment it's purely passing through to `anstream`), but I think it's more robust if we consolidate our color-choosing logic; this will be needed in the next PR that adds support for `FORCE_COLOR` (see below).

TODO:

- [ ] test on Windows 10
- [ ] investigate changed test snapshots with invalid characters

### Why doesn't this PR already support `FORCE_COLOR`?

Anstream doesn't have any built-in support for `FORCE_COLOR`. `owo-colors` has optional support for it, via the `supports-color` feature, but this requires using a less convenient (and likely less efficient) `if-supports-color` API. And doing color support selection in `owo-colors` is irrelevant if we are using `anstream`, because if `anstream` doesn't recognize `FORCE_COLOR` itself, it'll just happily strip out the color codes anyway. So my plan for `FORCE_COLOR` is to add a check for it in `ruff_linter::colors::choice` function, forcing it to return "yes colors" regardless of terminal. But I think it's clearer to add that behavior change in a separate PR.

## Test Plan

Existing test suite passes.

I also verified that `cargo run -- check ...` does have colored output in my terminal by default,  does not have colored output if I set `NO_COLOR=1`, does not have colored output if I set `CLICOLOR=0`, does not have colored output if I pipe to `less`, and does have colored output if I set `CLICOLOR_FORCE=1` and pipe to `less`.

<!-- How was it tested? -->


---

_Comment by @codspeed-hq[bot] on 2024-04-05 00:15_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/colors)

### Merging #10784 will **not alter performance**

<sub>Comparing <code>cjm/colors</code> (ab9b38c) with <code>main</code> (c2790f9)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-04-05 00:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh reviewed on 2024-04-05 00:31_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2510_invalid_characters.py.snap`:61 on 2024-04-05 00:31_

Trying to understand, why did the outer `}` get stripped here?

---

_Review comment by @charliermarsh on `docs/faq.md`:628 on 2024-04-05 00:33_

I think `anstream` also supports `CLICOLOR` and `CLICOLOR_FORCE`: https://github.com/rust-cli/anstyle/blob/ed1a598cc2c3b84483c6058a982d045f7f7aecfc/crates/anstyle-query/src/lib.rs#L34

---

_@charliermarsh reviewed on 2024-04-05 00:33_

---

_@carljm reviewed on 2024-04-05 00:34_

---

_Review comment by @carljm on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2510_invalid_characters.py.snap`:61 on 2024-04-05 00:34_

Heh, I was just editing my comment about this in the PR summary. I don't quite understand this either yet. These are tests of files with invalid/non-printable characters. Locally I thought what was happening was that anstream was just stripping the invalid chars, but looking at it now in the github diff it looks different. So I need to investigate this a bit more tomorrow.

---

_@carljm reviewed on 2024-04-05 00:35_

---

_Review comment by @carljm on `docs/faq.md`:628 on 2024-04-05 00:35_

Oh good point, I'll restore that paragraph.

---

_@charliermarsh reviewed on 2024-04-05 00:37_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/colors.rs`:19 on 2024-04-05 00:37_

(This location makes sense to me.)

---

_@charliermarsh reviewed on 2024-04-05 00:38_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/logging.rs`:158 on 2024-04-05 00:38_

For context, what's the second argument here? A separator?

---

_@charliermarsh reviewed on 2024-04-05 00:39_

---

_Review comment by @charliermarsh on `docs/faq.md`:628 on 2024-04-05 00:39_

Honestly had to dig through the source to know for sure (as you can tell)!

---

_@charliermarsh reviewed on 2024-04-05 00:39_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2510_invalid_characters.py.snap`:61 on 2024-04-05 00:39_

Ohh interesting! Yeah I did see the PR summary prior to writing this, but was trying to figure out what's up here.

---

_Comment by @charliermarsh on 2024-04-05 00:40_

This looks good to me! I'll hold off on approving while we figure out what's up with those snapshots.

---

_Comment by @charliermarsh on 2024-04-05 00:41_

The anstream docs do say:

> (windows) Falling back to the wincon API where [ENABLE_VIRTUAL_TERMINAL_PROCESSING](https://learn.microsoft.com/en-us/windows/console/console-virtual-terminal-sequences#output-sequences) is unsupported

I don't fully understand what that means, but it gives me hope that it does support Windows 10.


---

_@carljm reviewed on 2024-04-05 00:43_

---

_Review comment by @carljm on `crates/ruff_linter/src/logging.rs`:158 on 2024-04-05 00:43_

Yeah, this is fern's default line separator, so this isn't actually changing anything. But you have to specify it explicitly if you want to create your own fern::Output with your own writer. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2513_invalid_characters.py.snap`:8 on 2024-04-05 08:17_

This kind of makes sense to me. I suspect that it is insta who renders the `ESC` symbol for the whitespace only character. So the problem now is that `anstream` filters the characters out before they're passed to `anstream`. 

This goes into the "show fixes" project but I think we should improve our code frames to replace non-printable characters with printable characters before printing. 
https://github.com/biomejs/biome/blob/5505a1a0c7302f6aee1a9642bc83ed064e7820ea/crates/biome_diagnostics/src/display/frame.rs#L305-L406

I haven't checked if there's a Rust library that does that. 

I'm okay merging this PR and creating an issue for this instead. I actually kind of like the output now better because I suspect it now shows what users see in the terminal. @carljm could you test if what the snapshot shows matches Ruff's (main) output when run on the console?

---

_@MichaReiser approved on 2024-04-05 08:32_

This looks good to me and I like the `Colors::none` in tests. Much cleaner than my feature flag hack. 

Would you mind running the CPython hyperfine benchmark and compare main with your changes. I remember that there was a perf regression when I worked on this last time (my solution was different because I used `if_supports_colors`. I'm fine with a perf regression. I only want to understand the impact of the change. 

See https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#cpython-benchmark

I'm sorry, but I only have a Win 11 installation. I added an inline comment regarding the snapshot changes. I think the changes are good because they match what users see when running Ruff (what we show in snapshots today is a lie! It's not what our users see; we've been tricked by insta). 

```
cargo run --bin ruff -- check --select PLE /home/micha/astral/ruff/crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py --no-cache --preview
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff check --select PLE /home/micha/astral/ruff/crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py --no-cache --preview`
crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:15:6: PLE2510 [*] Invalid unescaped character backspace, use "\b" instead
   |
13 | #        (Pylint, "C3002") => Rule::UnnecessaryDirectLambdaCall,
14 | #foo = 'hi'
15 | b = '
   |       PLE2510
16 | b = f'
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:16:7: PLE2510 [*] Invalid unescaped character backspace, use "\b" instead
   |
14 | #foo = 'hi'
15 | b = '
16 | b = f'
   |        PLE2510
17 | 
18 | b_ok = '\\b'
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:24:12: PLE2512 [*] Invalid unescaped character SUB, use "\x1A" instead
   |
22 | cr_ok = f'\\r'
23 | 
24 | sub = 'sub '
   |             PLE2512
25 | sub = f'sub '
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:25:13: PLE2512 [*] Invalid unescaped character SUB, use "\x1A" instead
   |
24 | sub = 'sub '
25 | sub = f'sub '
   |              PLE2512
26 | 
27 | sub_ok = '\x1a'
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:30:16: PLE2513 [*] Invalid unescaped character ESC, use "\x1B" instead
   |
28 | sub_ok = f'\x1a'
29 | 
30 | esc = 'esc esc 
   |                 PLE2513
31 | esc = f'esc esc 
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:31:17: PLE2513 [*] Invalid unescaped character ESC, use "\x1B" instead
   |
30 | esc = 'esc esc 
31 | esc = f'esc esc 
   |                  PLE2513
32 | 
33 | esc_ok = '\x1b'
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:37:5: PLE2514 [*] Invalid unescaped character NUL, use "\0" instead
   |
36 | nul = '''
37 | nul '''
   |      PLE2514
38 | nul = f'''
39 | nul '''
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:39:5: PLE2514 [*] Invalid unescaped character NUL, use "\0" instead
   |
37 | nul '''
38 | nul = f'''
39 | nul '''
   |      PLE2514
40 | 
41 | nul_ok = '\0'
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:44:13: PLE2515 [*] Invalid unescaped character zero-width-space, use "\u200B" instead
   |
42 | nul_ok = f'\0'
43 | 
44 | zwsp = 'zero​width'
   |              PLE2515
45 | zwsp = f'zero​width'
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:45:14: PLE2515 [*] Invalid unescaped character zero-width-space, use "\u200B" instead
   |
44 | zwsp = 'zero​width'
45 | zwsp = f'zero​width'
   |               PLE2515
46 | 
47 | zwsp_ok = '\u200b'
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:50:36: PLE2515 [*] Invalid unescaped character zero-width-space, use "\u200B" instead
   |
48 | zwsp_ok = f'\u200b'
49 | 
50 | zwsp_after_multibyte_character = "ಫ​"
   |                                     PLE2515
51 | zwsp_after_multibyte_character = f"ಫ​"
52 | zwsp_after_multicharacter_grapheme_cluster = "ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:51:37: PLE2515 [*] Invalid unescaped character zero-width-space, use "\u200B" instead
   |
50 | zwsp_after_multibyte_character = "ಫ​"
51 | zwsp_after_multibyte_character = f"ಫ​"
   |                                      PLE2515
52 | zwsp_after_multicharacter_grapheme_cluster = "ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
53 | zwsp_after_multicharacter_grapheme_cluster = f"ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:52:60: PLE2515 [*] Invalid unescaped character zero-width-space, use "\u200B" instead
   |
50 | zwsp_after_multibyte_character = "ಫ​"
51 | zwsp_after_multibyte_character = f"ಫ​"
52 | zwsp_after_multicharacter_grapheme_cluster = "ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
   |                                                         PLE2515
53 | zwsp_after_multicharacter_grapheme_cluster = f"ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:52:61: PLE2515 [*] Invalid unescaped character zero-width-space, use "\u200B" instead
   |
50 | zwsp_after_multibyte_character = "ಫ​"
51 | zwsp_after_multibyte_character = f"ಫ​"
52 | zwsp_after_multicharacter_grapheme_cluster = "ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
   |                                                         PLE2515
53 | zwsp_after_multicharacter_grapheme_cluster = f"ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:53:61: PLE2515 [*] Invalid unescaped character zero-width-space, use "\u200B" instead
   |
51 | zwsp_after_multibyte_character = f"ಫ​"
52 | zwsp_after_multicharacter_grapheme_cluster = "ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
53 | zwsp_after_multicharacter_grapheme_cluster = f"ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
   |                                                          PLE2515
54 | 
55 | nested_fstrings = f{f'{f'}'}'
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:53:62: PLE2515 [*] Invalid unescaped character zero-width-space, use "\u200B" instead
   |
51 | zwsp_after_multibyte_character = f"ಫ​"
52 | zwsp_after_multicharacter_grapheme_cluster = "ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
53 | zwsp_after_multicharacter_grapheme_cluster = f"ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
   |                                                          PLE2515
54 | 
55 | nested_fstrings = f{f'{f'}'}'
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:55:21: PLE2510 [*] Invalid unescaped character backspace, use "\b" instead
   |
53 | zwsp_after_multicharacter_grapheme_cluster = f"ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
54 | 
55 | nested_fstrings = f{f'{f'}'}'
   |                      PLE2510
56 | 
57 | # https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741998106
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:55:25: PLE2512 [*] Invalid unescaped character SUB, use "\x1A" instead
   |
53 | zwsp_after_multicharacter_grapheme_cluster = f"ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
54 | 
55 | nested_fstrings = f{f'{f'}'}'
   |                         PLE2512
56 | 
57 | # https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741998106
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:55:29: PLE2513 [*] Invalid unescaped character ESC, use "\x1B" instead
   |
53 | zwsp_after_multicharacter_grapheme_cluster = f"ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
54 | 
55 | nested_fstrings = f{f'{f'}'}'
   |                            PLE2513
56 | 
57 | # https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741998106
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:58:12: PLE2512 [*] Invalid unescaped character SUB, use "\x1A" instead
   |
57 | # https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741998106
58 | x = f"""}}ab"""
   |             PLE2512
59 | # https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741998256
60 | x = f"""}}a"""
   |
   = help: Replace with escape sequence

crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py:60:12: PLE2513 [*] Invalid unescaped character ESC, use "\x1B" instead
   |
58 | x = f"""}}ab"""
59 | # https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741998256
60 | x = f"""}}a"""
   |             PLE2513
   |
   = help: Replace with escape sequence

Found 21 errors.
[*] 21 fixable with the `--fix` option.
```

Note: We use labels to generate the changelog. Labeling our PRs helps the person doing the release tremendously. I added the `internal` label because this is an internal refactor that, hopefully, isn't visible to users. You might want to change the label to CLI if there's some observable difference for users (e.g. do we support new environment variables?)

---

_Label `internal` added by @MichaReiser on 2024-04-05 08:33_

---

_@charliermarsh approved on 2024-04-05 21:18_

---

_Comment by @carljm on 2024-04-05 22:53_

It does look like this is a regression; I presume that's because of the extra stream filtering done by `anstream`:

```
ruff on main [$] via 𝗥 v1.77.1
➜ hyperfine --warmup 10 "./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ --no-cache -e"
Benchmark 1: ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ --no-cache -e
  Time (mean ± σ):     129.7 ms ±   1.6 ms    [User: 1119.6 ms, System: 75.5 ms]
  Range (min … max):   127.1 ms … 132.6 ms    22 runs

ruff on cjm/colors [⇡$] via 𝗥 v1.77.1
➜ hyperfine --warmup 10 "./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ --no-cache -e"
Benchmark 1: ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ --no-cache -e
  Time (mean ± σ):     144.7 ms ±   1.5 ms    [User: 1131.5 ms, System: 84.2 ms]
  Range (min … max):   141.8 ms … 147.0 ms    20 runs
```

That's pretty significant; it's like 11%.

Do we still want to go ahead with this change? I think we have two alternatives:

1. Switch to `owo-colors` but don't use `anstream`, just use `owo-colors` and `if-supports-color`. It sounds like that might also be a regression, but maybe a smaller one?
2. Stick with `colored` and just add support for `FORCE_COLOR` by checking the env var and setting a global override using `colored::control`. I presume this would be zero regression.

I can also dig into the cause of the regression more by generating a profile, but I'm not optimistic there will be much we can reasonably do to reduce the regression while still using the approach in this PR.

It's not clear to me if there are strong motivations to move off of `colored`, but I don't think it's necessary in order to support `FORCE_COLOR`; 11% seems like a lot of perf to give up.

@charliermarsh @MichaReiser

---

_Comment by @charliermarsh on 2024-04-06 15:21_

Oh wow. I'm surprised it's that significant -- thanks for benchmarking! (Maybe @MichaReiser will be less surprised?) I agree that 11% is way too much for something that's largely internal convenience with no user-facing benefit. Sorry for sending you on this path... That's my fault.

For context, the motivation for moving to `owo-colors` is that we use it in `uv` and we generally like the API a bit more than `colored`. I also thought it supported `FORCE_COLOR` out-of-the-box, so it seemed like a good excuse to migrate, but I since realized that I misread the comments on the other issue.

My vote would be to just implement `FORCE_COLOR` manually and close this for now. If you remove `anstream`, you'll then need to spend more time on this migration for questionable benefit, and we still won't be aligned with `uv`, which reduces the overall value.


---

_Comment by @MichaReiser on 2024-04-07 07:39_

I'm not surprised. I don't remember the exact numbers but I know it was significant enough that I didn't dare to put up a PR ;) (although my solution wasn't as good as @carljm's). 

I prefer to create a build of both versions and run hyperfine as a single command to reduce the chance that any other programs (or caches) influence the benchmark. I do this by building main and copy it to `./target/release/ruff-main` (It allows me to iterate on the branch version). But I do get the same numbers and the regression is worse when running in cached mode (not surprising)

```
hyperfine \
          --warmup 10 \
          --runs 100 \
          "./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ --no-cache -e" \
          "./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ --no-cache -e"
Benchmark 1: ./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ --no-cache -e
  Time (mean ± σ):     304.9 ms ±  15.6 ms    [User: 1608.4 ms, System: 76.1 ms]
  Range (min … max):   293.7 ms … 389.4 ms    100 runs
 
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.
 
Benchmark 2: ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ --no-cache -e
  Time (mean ± σ):     324.9 ms ±   7.0 ms    [User: 1616.1 ms, System: 84.4 ms]
  Range (min … max):   315.6 ms … 360.9 ms    100 runs
 
Summary
  ./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ --no-cache -e ran
    1.07 ± 0.06 times faster than ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ --no-cache -e

hyperfine \
          --warmup 10 \
          --runs 100 \
          "./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ -e" \
          "./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ -e"
Benchmark 1: ./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ -e
  Time (mean ± σ):      76.9 ms ±   0.9 ms    [User: 63.9 ms, System: 50.0 ms]
  Range (min … max):    75.3 ms …  79.3 ms    100 runs
 
Benchmark 2: ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ -e
  Time (mean ± σ):      96.6 ms ±   1.0 ms    [User: 74.6 ms, System: 58.9 ms]
  Range (min … max):    94.9 ms … 102.3 ms    100 runs
 
Summary
  ./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ -e ran
    1.26 ± 0.02 times faster than ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ -e
```

It seems that the `AutoStream::auto` method is somewhat involved. I list a few ideas without having looked at the code (I'm on the train and have very bad connectivity :( ). Can you change the code to always create a non colored stream to test if that changes the runtime in anyway. If it does, then we know that determining whether the terminal is colored or not is expensive and we should look into caching (`owo-colors` has a caching mechanism) or ensure that we move the stream creation out of any loop (if that's the case today). If the performance remains unchanged. Try always to create a colored stream. Does this change performance? If neither of the two helps. Are we locking the output stream the same as we did before (see https://docs.rs/anstream/latest/anstream/struct.AutoStream.html#method.lock)? I recommend limiting the investigation to 1-2h.

CPython is an edge case because it is a project with many diagnostics and Ruff prints all the diagnostics (it creates a 2MB diagnostic file using concise formatting). I strongly suspect that the slowest part of running ruff is my terminal rendering this wall of text. Do we see any regression when linting a project that has adopted Ruff (e.g. airflow?). I'm undecided how much the perf regression should bother me. I suspect that this use case will get slower when we switch to `output-format=full` and again when we start showing diffs for fixes and escape non-printable characters in code frames and diffs (which we currently print and who knows what that does to the user's terminal). Now, that's a bad excuse for an internal refactor making Ruff slower, even if it is an edge case. So maybe yes, keeping `colored` is still the best solution today.

Long term, I would love to see us having our own `ruff_markup` and `ruff_console` crates similar to biome ([`biom_markup`](https://github.com/biomejs/biome/tree/main/crates/biome_markup) and [`biome_console`](https://github.com/biomejs/biome/tree/main/crates/biome_console)) that abstracts the whole console output with a markdown. But that's slightly out of scope ;)


@carljm would you mind creating an issue that the code frame and diff rendering should escape non-visible characters? 

---

_Comment by @charliermarsh on 2024-04-07 16:04_

Reading Micha's comment: I think it makes sense to do some basic testing to understand whether (1) the terminal detection is a contributor to the slowness (hardcode to always use a non-colored stream), and then (2) whether the stream-stripping is a contributor to the slowness (hardcode to always use a colored stream, which presumedly is zero-cost). But yeah, let's timebox it to an hour or so, since it seems unlikely to ship right now.

---

_Comment by @carljm on 2024-04-08 20:17_

Yeah, confirmed that the regression doesn't improve at all if we force no-color (always filter the streams):

```
ruff on  cjm/colors [⇡$!] via 𝗥 v1.77.1 took 9s
➜ git di
diff --git a/crates/ruff_linter/src/colors.rs b/crates/ruff_linter/src/colors.rs
index 946216464..89e6c7073 100644
--- a/crates/ruff_linter/src/colors.rs
+++ b/crates/ruff_linter/src/colors.rs
@@ -6,6 +6,7 @@ pub fn none<S: RawStream>(stream: S) -> AutoStream<S> {
 }

 pub fn auto<S: RawStream>(stream: S) -> AutoStream<S> {
+    return none(stream);
     let choice = choice(&stream);
     AutoStream::new(stream, choice)
 }

ruff on  cjm/colors [⇡$!] via 𝗥 v1.77.1 took 41s
➜ hyperfine --warmup 10 "./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ -e" "./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ -e"
Benchmark 1: ./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ -e
  Time (mean ± σ):      66.0 ms ±   0.7 ms    [User: 53.0 ms, System: 61.1 ms]
  Range (min … max):    64.6 ms …  68.1 ms    44 runs

Benchmark 2: ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ -e
  Time (mean ± σ):      80.6 ms ±   0.6 ms    [User: 61.3 ms, System: 69.0 ms]
  Range (min … max):    79.4 ms …  82.6 ms    36 runs

Summary
  ./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ -e ran
    1.22 ± 0.02 times faster than ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ -e

ruff on  cjm/colors [⇡$!] via 𝗥 v1.77.1 took 8s
➜ hyperfine --warmup 10 "./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ --no-cache -e" "./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ --no-cache -e"
Benchmark 1: ./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ --no-cache -e
  Time (mean ± σ):     131.5 ms ±   4.4 ms    [User: 1116.9 ms, System: 75.5 ms]
  Range (min … max):   127.4 ms … 146.3 ms    20 runs

Benchmark 2: ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ --no-cache -e
  Time (mean ± σ):     145.7 ms ±   3.4 ms    [User: 1127.2 ms, System: 84.2 ms]
  Range (min … max):   141.3 ms … 155.1 ms    20 runs

Summary
  ./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ --no-cache -e ran
    1.11 ± 0.05 times faster than ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ --no-cache -e
```

And it (mostly) disappears if we run with color always on (no stream filtering):
```

ruff on  cjm/colors [⇡$!] via 𝗥 v1.77.1 took 9s
➜ git di
diff --git a/crates/ruff_linter/src/colors.rs b/crates/ruff_linter/src/colors.rs
index 946216464..cca490ed2 100644
--- a/crates/ruff_linter/src/colors.rs
+++ b/crates/ruff_linter/src/colors.rs
@@ -7,7 +7,7 @@ pub fn none<S: RawStream>(stream: S) -> AutoStream<S> {

 pub fn auto<S: RawStream>(stream: S) -> AutoStream<S> {
     let choice = choice(&stream);
-    AutoStream::new(stream, choice)
+    AutoStream::new(stream, ColorChoice::Always) //choice)
 }

 pub fn choice<S: RawStream>(stream: &S) -> ColorChoice {

ruff on  cjm/colors [⇡$!] via 𝗥 v1.77.1 took 42s
➜ hyperfine --warmup 10 "./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ -e" "./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ -e"
Benchmark 1: ./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ -e
  Time (mean ± σ):      63.9 ms ±   0.5 ms    [User: 51.1 ms, System: 60.9 ms]
  Range (min … max):    62.8 ms …  65.2 ms    45 runs

Benchmark 2: ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ -e
  Time (mean ± σ):      64.8 ms ±   0.6 ms    [User: 51.9 ms, System: 60.9 ms]
  Range (min … max):    63.8 ms …  66.1 ms    45 runs

Summary
  ./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ -e ran
    1.01 ± 0.01 times faster than ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ -e

ruff on  cjm/colors [⇡$!] via 𝗥 v1.77.1 took 8s
➜ hyperfine --warmup 10 "./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ --no-cache -e" "./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ --no-cache -e"
Benchmark 1: ./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ --no-cache -e
  Time (mean ± σ):     131.2 ms ±   4.9 ms    [User: 1123.2 ms, System: 74.8 ms]
  Range (min … max):   127.1 ms … 145.6 ms    23 runs

Benchmark 2: ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ --no-cache -e
  Time (mean ± σ):     133.4 ms ±   5.1 ms    [User: 1121.9 ms, System: 76.6 ms]
  Range (min … max):   129.0 ms … 147.1 ms    22 runs

Summary
  ./target/release/ruff-main ./crates/ruff_linter/resources/test/cpython/ --no-cache -e ran
    1.02 ± 0.05 times faster than ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ --no-cache -e
```

I think this confirms that the `anstream` approach to this problem, while conceptually nice, is just inherently pretty slow.

If we really do like the `owo-colors` API better than `colored`, I'm still open to adjusting this PR to remove `anstream` and just use `owo-colors` with `if-supports-color`, while I have all this context paged in. But based on Micha's previous experience, I might do that work and find it's still too much regression (though I'm curious where the regression actually comes from in that case.)

For now I'll close this and put up a separate PR to just add `FORCE_COLOR` support with no library changes.

I'll also open a separate issue about escaping non-printable chars in code frames / diffs.

---

_Closed by @carljm on 2024-04-08 20:18_

---

_Comment by @carljm on 2024-04-08 23:46_

Just in case anyone comes back to this PR in future, I wanted to also record some of my discoveries about the invalid-characters behavior here.

It is sort of true that this PR improves consistency between what users see and what insta sees, but only if the user is running in a no-color environment, so that anstream strips their output (like it does in tests). If the user is running in a color environment (probably the more common scenario), then this PR actually increases the divergence between what the user sees and what we see in `insta`, because the color-using user will see the somewhat-unpredictable results of directly outputting the control char, where insta will stop seeing the control char at all (because anstream strips it.)

Additionally, anstream's stripping is kind of unpredictable in the face of `\x1b` / `^[` / `ESC`, which makes sense, since that is the prefix for ANSI color codes as well. Depending on the exact next characters in the string after the `ESC` char, anstream might end up stripping one or two additional characters after the `ESC` char; this is the cause of the cases in this PR where we have an entire `'}` go missing.

So I don't think the current behavior of this PR is something we'd want to merge, and I'm honestly not sure what we _should_ do regarding non-printable chars (escaping them seems reasonable, but oddly neither `diff` nor `git diff` do that, and I think we should understand why before we diverge from their behavior), but I filed #10841 for further consideration.

---

_Comment by @zanieb on 2024-04-08 23:49_

Minor curiosity... why didn't Codspeed show this performance regression?

---

_Comment by @charliermarsh on 2024-04-09 00:00_

I don't know the answer, but IIRC CodSpeed can't / doesn't measure the cost of allocations, so if this the regression came from increased allocations, that wouldn't have shown up.

---

_Comment by @carljm on 2024-04-09 00:01_

Spot-checking some of the benchmarks that Codspeed runs, it looks like they are library benchmarks, not full sub-process benchmarks (i.e. they benchmark calling some library function with some data), so I think they are completely blind to the performance of console I/O.

I also grepped `crates/ruff_benchmarks/` for `Vec::new`, looking for any benchmarks that construct a bytes-vector-backed writer and pass it in to some console-output-emitting methods, but I didn't find any cases of that either. We could probably add a benchmark that does this, and if we had that it _probably_ would have caught the regression on this PR (presuming I caught the need to add a `color::none()` wrapper on that writer in that benchmark; if the benchmark also validated the output against a snapshot I probably would have).

---

_Comment by @charliermarsh on 2024-04-09 00:02_

> Spot-checking some of the benchmarks that Codspeed runs, it looks like they are library benchmarks, not full sub-process benchmarks (i.e. they benchmark calling some library function with some data), so I think they are completely blind to the performance of console I/O.

Oh yeah, that's interesting.

---

_Comment by @carljm on 2024-04-09 00:06_

> IIRC CodSpeed can't / doesn't measure the cost of allocations, so if this the regression came from increased allocations, that wouldn't have shown up.

That could be (I guess CodSpeed just measures CPU instructions?). But I would suspect that at least some of the cost here has to be actually scanning the strings for the ANSI escape codes (that can't be free!), and I would think CPU instrumentation would catch that.

---

_Comment by @epage on 2024-06-04 00:58_

Some quick thoughts for anyone that comes back to this
- It looks like the performance is purely with stripping and not overhead from choosing the stream type (since it stayed about the same with force-none and went away with force-always)
  - How much of a concern is performance for "piping to a file" (`none`) vs "showing to the user (usually `always`)
  - If people want to give a reproduction case they want me to look into, I can see if there are more gains, particularly geared to your workflow.  When I first created `anstream`, I knew performance for stripping was going to be a concern and specially optimized it compared to the [more popular stripping implementations](https://crates.io/crates/strip-ansi-escapes) but I was limited in my test cases and was focused on microbenchmarks (since this was before `anstream::AutoStream` existed)
- I recommend centralizing color definitions, e.g. https://github.com/rust-lang/cargo/blob/master/src/cargo/util/style.rs
- With the above and with `anstyle` API improvements (e.g. rust-lang/cargo#13368), I've been finding `anstyle` is sufficient for my needs for styling and there isn't a reason to pull in a separate library (`anstyle` is a dependency of `anstream`)
- If there are concerns for escape codes, feel free to start a discussion on anstyle repo
- I recommend trying out [anstyle-svg](https://docs.rs/anstyle-svg/latest/anstyle_svg/) for snapshotting of ANSI escape codes.  `snapbox`, CLI focused snapshotting alternative to `insta` that is being pulled out of cargo, has native support for this.

---

_Comment by @MichaReiser on 2024-06-04 06:43_

Hi @epage 

It's an honor to see you chime in on this discussion. 

Regarding using `anstyle` directly. I assume what you recommend here is to do something similar to the `annotated-snipped` crate (together with a `Stylesheet` that the `Renderer` uses):

https://github.com/rust-lang/annotate-snippets-rs/blob/master/src/renderer/display_list.rs#L201-L222

`anstyle-svg` sounds interesting. It certainly would be nice to capture color outputs in our tests.

---

_Comment by @epage on 2024-06-04 16:01_

imo for something like Ruff, you likely don't need the generalization that was done in `annotate-snippets` but could hard code things like Cargo did
- https://github.com/rust-lang/cargo/blob/master/src/cargo/util/style.rs
- https://github.com/rust-lang/cargo/pull/13368/files

While the original purpose of `anstyle` was to existing public APIs (like annotate-snippets or clap) and not as much to be used for styling. I think its been tweaked well enough to also work for styling and it was with that lens that I made the suggestion.

---
