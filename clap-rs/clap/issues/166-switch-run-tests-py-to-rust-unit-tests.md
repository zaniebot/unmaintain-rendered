```yaml
number: 166
title: Switch run_tests.py to Rust unit tests
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
assignees: []
created_at: 2015-07-27T12:23:47Z
updated_at: 2018-08-02T03:29:41Z
url: https://github.com/clap-rs/clap/issues/166
synced_at: 2026-01-12T16:14:08Z
```

# Switch run_tests.py to Rust unit tests

---

_@kbknapp_

Now with `get_matches_with()` we can simulate a CLI and should switch all our tests to Rust tests.

EDIT: Not all tests need to moved verbatim, the end result is that these things need to be tested either in the `lib.rs` `tests` module, or perhaps via a `tests` directory for integration tests...whichever is easier or more appropriate.
## Basics
- [x] flags using short
- [x] flags using long
- [x] flags using mixed (short and long)
- [x] multiple occurrences of flags (`-vvv` or `-v -v -v`)
- [x] multiple flags in single `-` (`-fBz` being equal to `-f -B -z`)
- [x] options with short
- [x] options with long and space (`--option value`)
- [x] options with long and equals (`--option=value`)
- [x] multiple values (`-o val1 val2`, **and** `-o val1 -o val2`)
- [x] minimum values (i.e. no less than 2 values)
- [x] maximum values (i.e. no more than 2 values)
- [x] set number of values (i.e. exactly 3 values)
- [x] option with set possible values
- [x] positional 
- [x] positional multiple values (`val1 val2`)
- [x] positional minimum values (i.e. no less than 2 values)
- [x] positional maximum values (i.e. no more than 2 values)
- [x] positional set number of values
- [x] positional with set of possible values
- [x] positional with multiple occurrences and set possible values
- [x] help with short
- [x] help with long
- [x] version with short
- [x] version with long
- [x] subcommands
- [x] subcommand with args (i.e. all the above with subcommands)
- [x] subcommand `help`
## Suggestions
- [x] possible value suggestion
- [x] long suggestion
- [x] subcommand suggestion
## Groups
- [x] required arg group
- [x] optional arg group
## Requirements
- [x] arg requires another arg
- [x] arg requires group
## AppSettings
- [x] SubcommandsNegateReqs
- [x] SubcommandRequired
- [x] ArgRequiredElseHelp
- [x] GlobalVersion
- [x] VersionlessSubcommands
- [x] UnifiedHelpMessages
- [x] WaitOnError
- [x] SubcommandRequiredElseHelp
## Conflicts
- [x] arg conflicts with another arg
- [x] arg that defined conflict appears before conflicting arg at runtime
- [x] arg that defined conflict appears after conflicting arg at runtime
- [x] arg conflicts with group
- [x] posix compatible flags using short
- [x] posix compatible flags using longs
- [x] posix compatible options using longs
- [x] posix compatible options using longs with `=`
- [x] posix compatibe options using shorts
- [x] conflict takes precedence over required argumet
- [x] required argument overridden
- [x] conflict overridden by third party argument (`A` hard conflicts with`C` and overrides `B`, runtime `A C B`)


---

_Label `T: enhancement` added by @kbknapp on 2015-07-27 12:23_

---

_Label `C: tests` added by @kbknapp on 2015-07-27 12:23_

---

_Label `D: easy` added by @kbknapp on 2015-07-27 12:23_

---

_Label `P3: want to have` added by @kbknapp on 2015-07-27 12:23_

---

_Label `E: tedious` added by @kbknapp on 2015-07-27 12:24_

---

_Comment by @Vinatorul on 2015-08-18 11:20_

May be it will be better to add combo boxes to this issue and switch them to unit tests one by one?!


---

_Comment by @kbknapp on 2015-08-18 13:32_

Good idea, I'll add this this later today :+1:


---

_Referenced in [clap-rs/clap#177](../../clap-rs/clap/pulls/177.md) on 2015-08-18 20:10_

---

_Comment by @Vinatorul on 2015-08-20 06:51_

I think we also need to add tests for:
- conflict takes precedence over required argumet
- required argument overridden 
- conflict overridden by third argument (negative and positive tests, as we discussed at gitter)


---

_Comment by @kbknapp on 2015-08-20 13:53_

Added :+1: 


---

_Referenced in [clap-rs/clap#181](../../clap-rs/clap/pulls/181.md) on 2015-08-21 21:41_

---

_Referenced in [clap-rs/clap#223](../../clap-rs/clap/pulls/223.md) on 2015-09-05 21:32_

---

_Referenced in [clap-rs/clap#227](../../clap-rs/clap/pulls/227.md) on 2015-09-06 10:51_

---

_Referenced in [clap-rs/clap#358](../../clap-rs/clap/pulls/358.md) on 2015-12-10 06:47_

---

_Comment by @sru on 2016-01-10 18:49_

Most of the remaining tests in `run_tests.py` are in `tests/` directory, especially `tests/tests.rs`.
- Some tests of subcommands are lacking.
- There are no tests for suggestions.
- There is test for printing out help message, but it does not check explicitly for the `-h` and `--help` flag.
- No tests for version flag.
- `tests/tests.rs` is messy (~1000 lines). Tests in there could be split and put into other test files, and leave only the general ones.

If these are sorted out, we could say we completely switched to Rust tests.


---

_Referenced in [clap-rs/clap#381](../../clap-rs/clap/pulls/381.md) on 2016-01-13 17:07_

---

_Referenced in [clap-rs/clap#382](../../clap-rs/clap/pulls/382.md) on 2016-01-15 04:31_

---

_Closed by @homu on 2016-05-10 00:12_

---
