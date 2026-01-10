---
number: 1960
title: "Default `--version` doesn't include newline"
type: issue
state: closed
author: tailhook
labels:
  - C-bug
  - E-easy
assignees: []
created_at: 2020-06-04T11:10:17Z
updated_at: 2020-06-10T19:59:33Z
url: https://github.com/clap-rs/clap/issues/1960
synced_at: 2026-01-10T01:27:10Z
---

# Default `--version` doesn't include newline

---

_Issue opened by @tailhook on 2020-06-04 11:10_

In version 2.33.0: `cargo run --example 01a_quick_example -- --version` prints version with the final newline. In the current master (f4ace424) it doesn't include the newline which makes it inconvenient in some shells:
```
[root@8428a91f4e1e /]# myapp --version
MyApp 1.0[root@8428a91f4e1e /]#
```

I couldn't find an issue about that. Was the change intentional?

---

_Label `T: bug` added by @tailhook on 2020-06-04 11:10_

---

_Referenced in [geldata/gel-cli#76](../../geldata/gel-cli/issues/76.md) on 2020-06-04 11:11_

---

_Comment by @asteding on 2020-06-10 07:52_

Hello, 
@tailhook which shell do you use?

On my `zsh` or `bash` right now i cannot confirm the bug for version `2.33.0`

Best regards.

---

_Comment by @pksunkara on 2020-06-10 07:58_

He's talking about master

---

_Comment by @CreepySkeleton on 2020-06-10 08:14_

This reproduces on linux (bash and zsh) but not on windows (CMD)

---

_Label `P1: urgent` added by @CreepySkeleton on 2020-06-10 08:15_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-06-10 08:15_

---

_Label `D: easy` added by @pksunkara on 2020-06-10 08:15_

---

_Label `Z: good first issue` added by @pksunkara on 2020-06-10 08:15_

---

_Comment by @asteding on 2020-06-10 08:56_

Oh I see. Sorry my bad.

I still digged into the code and probably
```rust
    pub(crate) fn _write_version<W: Write>(&self, w: &mut W, use_long: bool) -> io::Result<()> {
        debug!("App::_write_version");

        let ver = if use_long {
            self.long_version
                .unwrap_or_else(|| self.version.unwrap_or(""))
        } else {
            self.version
                .unwrap_or_else(|| self.long_version.unwrap_or(""))
        };
        if let Some(bn) = self.bin_name.as_ref() {
            if bn.contains(' ') {
                // In case we're dealing with subcommands i.e. git mv is translated to git-mv
                write!(w, "{} {}", bn.replace(" ", "-"), ver)
            } else {
                write!(w, "{} {}", &self.name[..], ver)
            }
        } else {
            write!(w, "{} {}", &self.name[..], ver)
        }
    }
```

Is lacking some `\n` for the `write!()` macros isn't it? If i add them i get a new line in my bash.

---

_Comment by @pksunkara on 2020-06-10 08:58_

Yeah the fix is as simple as that. Please make you add a test or update one if it already exists.

---

_Comment by @asteding on 2020-06-10 12:24_

Okay so i think using a `writeln!(w, "{} {}", bn.replace(" ", "-"), ver)` instead of `write!(w, "{} {}", bn.replace(" ", "-"), ver)` is the best solution for this bug.

What I struggle with is a fitting test. Here's the test code i added to `tests/help.rs`

```rust
static ISSUE_1960_MISSING_NEWLINE: &str = "soapsurfer 1.2.3\n";
#[test]
fn issue_1960_missing_newline() {
    let app = App::new("soapsurfer").version("1.2.3");
    assert!(utils::compare_output(
        app,
        "soapsurfer --version",
        ISSUE_1960_MISSING_NEWLINE,
        false
    ))
}
```

But this test passes with and without the `\n` in `static ISSUE_1960_MISSING_NEWLINE: &str = "soapsurfer 1.2.3\n";` 

Any thoughts?


---

_Comment by @pksunkara on 2020-06-10 12:27_

The tests might be using `trim` for the newline char.

---

_Comment by @asteding on 2020-06-10 12:37_

Yes exaclty!
Nice, thanks a lot. It's probably not a good idea to add a parameter `trim` to `compare_output()` so i probably write new testing functions in `utils.rs`? Would that be fitting?

---

_Comment by @pksunkara on 2020-06-10 12:38_

Try removing `trim` from that function and see what functions fail.

---

_Comment by @asteding on 2020-06-10 12:50_

The result okay so far. It's only
```
failures:
    dont_collapse_args
    issue_1093_allow_ext_sc
    require_eq
    skip_possible_values
    unified_help
```
Looking at e.g. the definition of `dont_collapse_args` 
```rust
static DONT_COLLAPSE_ARGS: &str = "clap-test v1.4.8

USAGE:
    clap-test [arg1] [arg2] [arg3]

ARGS:
    <arg1>    some
    <arg2>    some
    <arg3>    some

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information";
```
There would now be the need to also add newline charater `\n` so:
```rust
static DONT_COLLAPSE_ARGS: &str = "clap-test v1.4.8

USAGE:
    clap-test [arg1] [arg2] [arg3]

ARGS:
    <arg1>    some
    <arg2>    some
    <arg3>    some

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information\n";
```

---

_Comment by @asteding on 2020-06-10 13:27_

It looks like a whole bunch of other test cases seems to be lacking `\n` at the end.
So what should I do?

edit: I probably found a good solution and i'm preparing the pull request so far.
Let's see what you guys think of it.

---

_Comment by @pksunkara on 2020-06-10 13:59_

If it's only a few tests you can add a new line to the fixtures

---

_Referenced in [clap-rs/clap#1972](../../clap-rs/clap/pulls/1972.md) on 2020-06-10 14:16_

---

_Closed by @bors[bot] on 2020-06-10 19:59_

---

_Referenced in [sharkdp/hexyl#131](../../sharkdp/hexyl/issues/131.md) on 2021-04-23 04:43_

---

_Referenced in [nervosnetwork/ckb-cli#417](../../nervosnetwork/ckb-cli/issues/417.md) on 2021-08-24 07:49_

---
