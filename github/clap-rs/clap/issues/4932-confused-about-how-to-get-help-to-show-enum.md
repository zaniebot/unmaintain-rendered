---
number: 4932
title: "Confused about how to get `--help` to show `enum` variants with `clap` v4 (Doc issue?)"
type: issue
state: closed
author: U007D
labels: []
assignees: []
created_at: 2023-05-22T11:15:36Z
updated_at: 2023-05-22T12:20:50Z
url: https://github.com/clap-rs/clap/issues/4932
synced_at: 2026-01-07T13:12:20-06:00
---

# Confused about how to get `--help` to show `enum` variants with `clap` v4 (Doc issue?)

---

_Issue opened by @U007D on 2023-05-22 11:15_

### Discussed in https://github.com/clap-rs/clap/discussions/4931

<div type='discussions-op-text'>

<sup>Originally posted by **U007D** May 22, 2023</sup>
I am using `clap` 4.3 and have an enum whose options are non-obvious.  I assumed that `clap` would list the valid options for this `enum` in help:

```rust
/// Supported target platforms.
#[derive(Clone, Copy, Debug, Parser, Eq, Hash, Ord, PartialEq, PartialOrd)]
pub enum Board {
    #[clap(about = "SiFive HiFive Freedom Unmatched 740 Development Board (RISC-V64, 1x-IMAC + 4x-GC)")]
    SiFiveFU740,
    #[clap(about = "ST Microelectronics 32-bit F3 Discovery Board (ARM Cortex M4)")]
    Stm32F3Discovery,
    // ...
}
```

but the variant names are not printed by default.  Reading through various issues, it seemed that `ArgEnum` was a solution introduced in `clap` v3, but `ArgEnum` appears to be gone in v4.  I saw a posting suggesting:

```rust
#[clap(value_parser = clap::builder::PossibleValuesParser::new(Board::VARIANTS).map(|s| s.parse::<Board>().unwrap()))]
pub enum Board {
    // ...
```

but this did not show the `Board` variants in `--help`.

It *seems* `clap` can be made to provide this information, but it is unclear to me how to do this under v4.  Can someone help explain how to do this?

Thanks,
U007D</div>

---

_Comment by @U007D on 2023-05-22 12:20_

Removing the `Parser` derive macro and replacing it with `ValueEnum` seems to work:
```rust
/// Supported target platforms.
#[derive(Clone, Copy, Debug, ValueEnum, Eq, Hash, Ord, PartialEq, PartialOrd)]
pub enum Board {
    /// SiFive HiFive Freedom Unmatched 740 Development Board (RISC-V64, 1x-IMAC + 4x-GC)
    SiFiveFU740,
    /// ST Microelectronics 32-bit F3 Discovery Board (ARM Cortex M4)
    Stm32F3Discovery,
}
```
In the argument struct add `value_enum` derive macro for `Board`:
```rust
#[derive(Clone, Debug, Parser, Eq, Hash, Ord, PartialEq, PartialOrd)]
pub enum Args {
     // ...
    /// Build the project, yielding a binary for the specified target platform.
    Build {
        /// Specify target platform.
        #[clap(short, long, value_enum)]
        board: Board,
        // ...
```

This yields:
```
▶ cargo xt flash --help board
   Compiling xtask v0.1.0 (/Users/Shared/Development/bg/ada_workspace/build_tools/xtask)
    Finished dev [optimized + debuginfo] target(s) in 0.68s
     Running `target/debug/xtask flash --help board`
Flash specified binary to target board (platform)

Usage: xtask flash [OPTIONS] --board <BOARD> <BINARY>

Arguments:
  <BINARY>
          Specify binary image to flash

Options:
  -a, --address <ADDRESS>
          Specify hexadecimal start address to flash to (`0x` prefix optional).  Default is board's flash memory base address

  -b, --board <BOARD>
          Specify target platform

          Possible values:
          - si-five-fu740:      SiFive HiFive Freedom Unmatched 740 Development Board (RISC-V64, 1x-IMAC + 4x-GC)
          - stm32-f3-discovery: ST Microelectronics 32-bit F3 Discovery Board (ARM Cortex M4)

  -v, --verbosity <VERBOSITY>
          Provide verbose output from build activities

          [possible values: true, false]

  -h, --help
          Print help (see a summary with '-h')
```


---

_Closed by @U007D on 2023-05-22 12:20_

---
