```yaml
number: 3108
title: "`-q` inverts status code from `--files-without-match`"
type: issue
state: closed
author: BatmanAoD
labels:
  - bug
  - rollup
assignees: []
created_at: 2025-07-24T16:24:46Z
updated_at: 2025-09-20T01:08:45Z
url: https://github.com/BurntSushi/ripgrep/issues/3108
synced_at: 2026-01-12T16:13:25Z
```

# `-q` inverts status code from `--files-without-match`

---

_@BatmanAoD_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0 (rev e50df40a19)

features:-simd-accel,+pcre2
simd(compile):+NEON
simd(runtime):+NEON

PCRE2 10.42 is available (JIT is available)

### How did you install ripgrep?

`cargo binstall`

### What operating system are you using ripgrep on?

macOS Seqouia 15.5, ARM

### Describe your bug.

The exit code from `rg -q --files-without-match` is inverted from the exit code from `rg --files-without-match`. As far as I can tell, this is not documented, so I don't believe it's intentional. There is a parenthetical in the `-q` helptext that says the exit code "will be an error code if no matches are found", but since it's a parenthetical, I interpret that as explaining the default exit code behavior.

If this is the intended behavior, I think that should be more explicit in the helptext; perhaps something like:

> The exit code will be an error code if no matches are found. This overrides other flags that may change the exit code, such as --files-without-match.

### What are the steps to reproduce the behavior?

```sh
$> echo haystack ...... | rg -q needle; echo $?
1
$> echo haystack needle | rg -q needle; echo $?
0
$> echo haystack ...... | rg --files-without-match needle > /dev/null; echo $?
0
$> echo haystack needle | rg --files-without-match needle > /dev/null; echo $?
1
$> echo haystack ...... | rg -q --files-without-match needle > /dev/null; echo $?
1
$> echo haystack needle | rg -q --files-without-match needle > /dev/null; echo $?
0
```


### What is the actual behavior?

`-q` inverts the exit code, effectively ignoring `--files-without-match`.

### What is the expected behavior?

I would expect `-q` not to change the exit code.

---

_Label `bug` added by @BurntSushi on 2025-07-24 16:29_

---

_Comment by @BurntSushi on 2025-07-24 16:29_

This is definitely a bug.

---

_Comment by @emrebengue on 2025-07-25 03:08_

I think the issue is the ExitCode logic in run function inside `crates/core/main.rs`. 

I do not quite understand why this behaviour happens. When you look at `ripgrep/crates/printer/src/summary.rs` you will see that has_match() correctly returns true IF config.kind is SummaryKind::PathWithoutMatch and match_count is 0. Which is the correct logic because it is expected to return true if the path/file does not contain a match. 

But this creates an issue when it comes to ExitCode logic.
Adding the following logic to crates/core/main.rs inside `run` function seems to fix the bug for now.

```
if args.mode() == Mode::Search(SearchMode::FilesWithoutMatch)
        && args.quiet()
    {
        matched = !matched;
    }
```

```
fn run(result: crate::flags::ParseResult<HiArgs>) -> anyhow::Result<ExitCode> {
    use crate::flags::{Mode, ParseResult};

    let args = match result {
        ParseResult::Err(err) => return Err(err),
        ParseResult::Special(mode) => return special(mode),
        ParseResult::Ok(args) => args,
    };
    let mut matched = match args.mode() {
        Mode::Search(_) if !args.matches_possible() => false,
        Mode::Search(mode) if args.threads() == 1 => search(&args, mode)?,
        Mode::Search(mode) => search_parallel(&args, mode)?,
        Mode::Files if args.threads() == 1 => files(&args)?,
        Mode::Files => files_parallel(&args)?,
        Mode::Types => return types(&args),
        Mode::Generate(mode) => return generate(mode),
    };
    if args.mode() == Mode::Search(SearchMode::FilesWithoutMatch)
        && args.quiet()
    {
        matched = !matched;
    }
    Ok(if matched && (args.quiet() || !messages::errored()) {
        ExitCode::from(0)
    } else if messages::errored() {
        ExitCode::from(2)
    } else {
        ExitCode::from(1)
    })
}
```

If someone can elaborate on this bug, it would be appreciated.

---

_Comment by @emrebengue on 2025-07-25 06:28_

It can be refactored as: 
```
fn run(result: crate::flags::ParseResult<HiArgs>) -> anyhow::Result<ExitCode> {
    use crate::flags::{Mode, ParseResult};

    let args = match result {
        ParseResult::Err(err) => return Err(err),
        ParseResult::Special(mode) => return special(mode),
        ParseResult::Ok(args) => args,
    };
    let matched = match args.mode() {
        Mode::Search(_) if !args.matches_possible() => false,
        Mode::Search(mode) if args.threads() == 1 => search(&args, mode)?,
        Mode::Search(mode) => search_parallel(&args, mode)?,
        Mode::Files if args.threads() == 1 => files(&args)?,
        Mode::Files => files_parallel(&args)?,
        Mode::Types => return types(&args),
        Mode::Generate(mode) => return generate(mode),
    };
    Ok(
        if exit_success(
            matched,
            &args.mode(),
            args.quiet(),
            messages::errored(),
        ) {
            ExitCode::from(0)
        } else if messages::errored() {
            ExitCode::from(2)
        } else {
            ExitCode::from(1)
        },
    )
}

fn exit_success(
    matched: bool,
    mode: &Mode,
    quiet: bool,
    errored: bool,
) -> bool {
    if errored {
        return false;
    }
    match mode {
        Mode::Search(SearchMode::FilesWithoutMatch) if quiet => !matched,
        _ => matched,
    }
}
```

---

_Comment by @wislertt on 2025-08-02 14:23_

@BurntSushi @emrebengue Fixed this issue in PR #3118. Would appreciate a review when you have a moment.

---

_Label `rollup` added by @BurntSushi on 2025-08-20 01:09_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
