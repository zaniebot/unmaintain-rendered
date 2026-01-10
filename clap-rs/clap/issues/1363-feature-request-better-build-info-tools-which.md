---
number: 1363
title: "Feature request: better build info tools (which features, codegen a binary was compiled with)"
type: issue
state: closed
author: jonathanstrong
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2018-10-18T01:12:20Z
updated_at: 2022-02-03T14:45:10Z
url: https://github.com/clap-rs/clap/issues/1363
synced_at: 2026-01-10T01:26:50Z
---

# Feature request: better build info tools (which features, codegen a binary was compiled with)

---

_Issue opened by @jonathanstrong on 2018-10-18 01:12_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.31.0-nightly (bef62ccdd 2018-10-16)

### Affected Version of clap

2.32.0

### Bug or Feature Request Summary

I often find myself checking which version of a binary I compiled, between versions, crate features and codegen options. I wrote some code below which I am using via the `App::about` method to provide build info for my clap-generated `-h` printout. (Like `vim --version`, but much less info).

```rust
pub fn build_info() -> String {
    let indent = "    ";
    format!("\r\n\
        BUILD:\r\n\
        {}{}\r\n\r\n\
        CODEGEN:\r\n\
        {}{}",
        indent, features().join(" "),
        indent, codegen().join(" "))
}

fn codegen() -> Vec<&'static str> {
    let mut flags = Vec::new();
    macro_rules! check_cpu {
        ($($target:expr),*) => {{
            $(
                #[cfg(target_feature = $target)]
                flags.push($target);
            )*
        }}
    }

    check_cpu!("aes", "avx", "avx2", "avx512bw", "avx512cd", "avx512dq", "avx512f", "avx512vl", "bmi1",
               "bmi2", "fma", "fxsr", "lzcnt", "mmx", "pclmulqdq", "popcnt", "rdrand", "rdseed", "sse",
               "sse2", "sse3", "sse4.1", "sse4.2", "ssse3", "xsave", "xsavec", "xsaveopt", "xsaves");

    flags.sort_unstable();
    flags.dedup();
    flags
}

fn features() -> Vec<&'static str> {
    macro_rules! feat_list {
        ($($feat:expr),*) => {
            vec![
                $(
                    #[cfg(feature = $feat)]
                    $feat,
                )*
            ]
        }
    }

    let mut flags = feat_list!("feat-one", "feat-two");

    #[cfg(debug_assertions)]
    flags.push("debug_assertions");

    flags.sort_unstable();
    flags.dedup();
    flags
}

// ...

fn main()
    let build_info = mylib::build_info();
    let args: ArgMatches = App::new("my-binary")
        .version(crate_version!())
        .about(build_info.as_str())
        // ...
```

This is ok, but unfortunately it needs to be manually maintained as new features are added, etc. It occurred to me that perhaps clap would be a good fit for this kind of functionality, and I didn't see anything in the docs about providing build info. Have you considered adding tools for tracking features, codegen options, etc.?

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 15:45_

---

_Label `D: intermediate` added by @CreepySkeleton on 2020-02-01 15:45_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 15:45_

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-02-01 15:45_

---

_Label `T: new feature` added by @CreepySkeleton on 2020-02-01 15:45_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-01 15:45_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-02-01 15:45_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [epage/clapng#106](../../epage/clapng/issues/106.md) on 2021-12-06 17:37_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:17_

---

_Comment by @epage on 2021-12-09 19:11_

This also has overlap with
- https://github.com/sharkdp/bugreport
- https://github.com/rust-cli/human-panic



---

_Removed from milestone `3.1` by @epage on 2021-12-09 19:11_

---

_Label `D: medium` removed by @epage on 2021-12-09 19:11_

---

_Label `E-help-wanted` removed by @epage on 2021-12-09 19:11_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 19:11_

---

_Comment by @epage on 2022-02-03 14:35_

https://crates.io/crates/shadow-rs seems to also fill a similar role
- https://github.com/baoyachi/shadow-rs/issues/69
- https://github.com/baoyachi/shadow-rs/issues/70

---

_Referenced in [baoyachi/shadow-rs#69](../../baoyachi/shadow-rs/issues/69.md) on 2022-02-03 14:39_

---

_Closed by @epage on 2022-02-03 14:45_

---
