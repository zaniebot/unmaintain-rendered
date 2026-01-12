```yaml
number: 9169
title: "Fuzz proposal: fuzz target for formatting"
type: issue
state: closed
author: manunio
labels:
  - question
  - formatter
  - fuzzer
assignees: []
created_at: 2023-12-17T15:22:17Z
updated_at: 2024-01-11T18:04:46Z
url: https://github.com/astral-sh/ruff/issues/9169
synced_at: 2026-01-12T15:54:49Z
```

# Fuzz proposal: fuzz target for formatting

---

_@manunio_

Hi, Would be interested in  a fuzz target that formates the code twice and checks for inconsistencies between them, something which is done here:
https://github.com/astral-sh/ruff/blob/c944d23053fe84e63a7515c75a50ce46b63dc2e0/crates/ruff_python_formatter/tests/fixtures.rs#L213

For example something like this:
```rust
#![no_main]

use libfuzzer_sys::{fuzz_target, Corpus};
use ruff_python_formatter::{format_module_source, PyFormatOptions};
use similar::TextDiff;


fn assert_difference(formatted: &str, reformatted: &str) {
    if reformatted != formatted {
        let diff = TextDiff::from_lines(formatted, reformatted)
            .unified_diff()
            .header("Formatted once", "Formatted twice")
            .to_string();
        panic!(
            r#"Reformatting the formatted code a second time resulted in formatting changes.
---
{diff}---

Formatted once:
---
{formatted}---

Formatted twice:
---
{reformatted}---"#,);
    }
}


fn do_fuzz(case: &[u8]) -> Corpus {
    // throw away inputs which aren't utf-8
    let Ok(code) = std::str::from_utf8(case) else {
        return Corpus::Reject;
    };

    // format code twice
    let formatted = match format_module_source(code, PyFormatOptions::default()) {
        Ok(formatted) => formatted,
        Err(_) => return Corpus::Reject,
    };

    let reformatted = match format_module_source(formatted.as_code(), PyFormatOptions::default()) {
        Ok(reformatted) => reformatted,
        Err(_) => return Corpus::Reject,
    };

    assert_difference(formatted.as_code(), reformatted.as_code());

    Corpus::Keep
}


fuzz_target!(|case: &[u8]| -> Corpus { do_fuzz(case) });
```

---

_Label `formatter` added by @MichaReiser on 2023-12-18 03:05_

---

_Label `fuzzer` added by @MichaReiser on 2023-12-18 03:05_

---

_Comment by @MichaReiser on 2023-12-18 03:18_

@qarmin did some fuzzing of the formatter in the past. See https://github.com/astral-sh/ruff/issues/7433

I don't know if the script they used is available somewhere. 

Overall,  we've found fuzzing a useful tool to improve Ruff's stability and it helped us find many stability issues before releasing ruff's formatter. However, fuzzers also discover issues that are very rare or close to non-existent in practice. That's why reporting and fixing all of them isn't necessary impactful for our users and is the reason why we prioritize real-world issues or new features over fuzzer issues (but this depends on the fuzzer issue).


What's most helpful for us is a list of triaged fuzzer issues. But I think triaging is hard to impossible without understanding how our formatter works. Having a way to run the fuzzer (by someone who can triage issues but doesn't know how fuzzing works) could, therefore, be helpful, if we can integrate it into our exsiting fuzzer infrastructure (see `fuzzer` directory).

---

_Label `question` added by @MichaReiser on 2023-12-18 03:19_

---

_Comment by @manunio on 2023-12-18 08:57_

> @qarmin did some fuzzing of the formatter in the past. See https://github.com/astral-sh/ruff/issues/7433
> 
> I don't know if the script they used is available somewhere. 
> 
> Overall,  we've found fuzzing a useful tool to improve Ruff's stability and it helped us find many stability issues before releasing ruff's formatter. However, fuzzers also discover issues that are very rare or close to non-existent in practice. That's why reporting and fixing all of them isn't necessary impactful for our users and is the reason why we prioritize real-world issues or new features over fuzzer issues (but this depends on the fuzzer issue).
> 
> 
> What's most helpful for us is a list of triaged fuzzer issues. But I think triaging is hard to impossible without understanding how our formatter works. Having a way to run the fuzzer (by someone who can triage issues but doesn't know how fuzzing works) could, therefore, be helpful, if we can integrate it into our exsiting fuzzer infrastructure (see `fuzzer` directory).

Hi @MichaReiser thanks for your reply, I'm a bit confused here do you want me to add above code in fuzz directory so that it can be used by traigers who knows about how formatter works but doesn't know how fuzzing works?

---

_Comment by @MichaReiser on 2023-12-18 09:55_

Yeah, go for it if you're interested in adding it to our existing fuzzer. 

---

_Comment by @manunio on 2023-12-22 18:33_

After running the above `fuzz_target` (which formats the code twice) for some time, I found a difference between two versions. In this case, the input gets parsed by the AST but throws a `NameError` in the REPL.

```sh
>>> print(ast.dump(ast.parse("z = V, z = z = V, zxV, = z = V, z = z = V, = z = V, VV,"), indent=4))
Module(
    body=[
        Assign(
            targets=[
                Name(id='z', ctx=Store()),
                Tuple(
                    elts=[
                        Name(id='V', ctx=Store()),
                        Name(id='z', ctx=Store())],
                    ctx=Store()),
                Name(id='z', ctx=Store()),
                Tuple(
                    elts=[
                        Name(id='V', ctx=Store()),
                        Name(id='zxV', ctx=Store())],
                    ctx=Store()),
                Name(id='z', ctx=Store()),
                Tuple(
                    elts=[
                        Name(id='V', ctx=Store()),
                        Name(id='z', ctx=Store())],
                    ctx=Store()),
                Name(id='z', ctx=Store()),
                Tuple(
                    elts=[
                        Name(id='V', ctx=Store())],
                    ctx=Store()),
                Name(id='z', ctx=Store())],
            value=Tuple(
                elts=[
                    Name(id='V', ctx=Load()),
                    Name(id='VV', ctx=Load())],
                ctx=Load()))],
    type_ignores=[])
>>>

```
```sh
❯ i="z = V, z = z = V, zxV, = z = V, z = z = V, = z = V, VV,";\
  echo "\nInput:\n $i";\
  ff=$(echo $i | ruff format -); echo "\nFirst Format:\n$ff";\
  sf=$(echo $ff | ruff format -); echo "\nSecond Format:\n$sf";


Input:
 z = V, z = z = V, zxV, = z = V, z = z = V, = z = V, VV,

First Format:
z = (
    V, z
) = (
    z
) = (
    (
        V,
        zxV,
    )
) = z = V, z = z = (V,) = z = (
    V,
    VV,
)

Second Format:
z = (V, z) = z = (
    V,
    zxV,
) = z = V, z = z = (V,) = z = (
    V,
    VV,
)

````

> However, fuzzers also discover issues that are very rare or close to non-existent in practice. That's why reporting and fixing all of them isn't necessary impactful for our users and is the reason why we prioritize real-world issues or new features over fuzzer issues (but this depends on the fuzzer issue).

@MichaReiser Did you mean this type of report?

The input `z = V, z = z = V, zxV, = z = V, z = z = V, = z = V, VV,` passes fine through the Ruff parser, Python's ast.parse(), and even works with py_compile, but it just throws an error at runtime.

---

_Comment by @addisoncrump on 2024-01-03 01:00_

Was this ever added? Formatter idempotency has proven to be super useful for bug detection in other projects (see @jasikpark's work with Prettier, my work with boa and Biome).

---

_Comment by @addisoncrump on 2024-01-03 11:53_

I slept on it; I actually think both this and a [format validity fuzzer](https://github.com/biomejs/biome/blob/main/fuzz/fuzz_targets/rome_common.rs#L61-L165) which [fuzzes to check that the formatter does not introduce linter errors](https://github.com/biomejs/biome/blob/main/fuzz/README.md#rome_format_) would be good to add.

---

_Comment by @addisoncrump on 2024-01-03 11:56_

@manunio: I think your fuzzer is a good start here, but the reformatting should never fail (instead should `panic!`) as we have a safety guarantee from the formatter to produce valid syntax. Do you want to open a PR? We can discuss further changes there.

---

_Comment by @manunio on 2024-01-03 13:06_

> @manunio: I think your fuzzer is a good start here, but the reformatting should never fail (instead should `panic!`) as we have a safety guarantee from the formatter to produce valid syntax. Do you want to open a PR? We can discuss further changes there.

Hi @addisoncrump thanks for the feedback, I'll open a pr here as soon as I have access to my system.

---

_Comment by @MichaReiser on 2024-01-08 08:23_

> I slept on it; I actually think both this and a [format validity fuzzer](https://github.com/biomejs/biome/blob/main/fuzz/fuzz_targets/rome_common.rs?rgh-link-date=2024-01-03T11%3A53%3A08Z#L61-L165) which [fuzzes to check that the formatter does not introduce linter errors](https://github.com/biomejs/biome/blob/main/fuzz/README.md?rgh-link-date=2024-01-03T11%3A53%3A08Z#rome_format_) would be good to add.

This sounds interesting. Especially to discover incompatibilities that aren't obvious. Although we may need to have a ignore list of rules that we know are not compatible. 

> Was this ever added? Formatter idempotency has proven to be super useful for bug detection in other projects (see @jasikpark's work with Prettier, my work with boa and Biome).

Honestly, I'm not sure but I think it would be valuable to have. 

---

_Comment by @manunio on 2024-01-11 18:04_

Closing this now as https://github.com/astral-sh/ruff/pull/9448 has been merged.

---

_Closed by @manunio on 2024-01-11 18:04_

---
