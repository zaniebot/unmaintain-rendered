```yaml
number: 2468
title: Support non-Unix options (e.g.  find -exec, bash +O shopt, cmd.exe /c, gcc -Wl,)
type: issue
state: open
author: mkayaalp
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-mentor
assignees: []
created_at: 2021-05-05T10:44:51Z
updated_at: 2022-11-29T12:41:32Z
url: https://github.com/clap-rs/clap/issues/2468
synced_at: 2026-01-12T16:14:13Z
```

# Support non-Unix options (e.g.  find -exec, bash +O shopt, cmd.exe /c, gcc -Wl,)

---

_@mkayaalp_

Child Tasks:
- [ ] #3026 
- [ ] #1210 (if we generalize it to this case)

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Describe your use case

There are many CLIs that do not follow the Unix way of short vs. long options. While it might not be a good idea for new programs to have such an interface, it may be needed for compatibility.

## Examples
### 1. Long option with a single hyphen-minus (`-`) instead of a double hyphen-minus (`--`)
The examples are probably too many to count, as this is very common. From the output of `find --help`:
```
      -ilname PATTERN -iname PATTERN -inum N -iwholename PATTERN -iregex PATTERN
      -links N -lname PATTERN -mmin N -mtime N -name PATTERN -newer FILE
```
From the output of `gcc --help`:
```
  -dumpspecs               Display all of the built in spec strings.
  -dumpversion             Display the version of the compiler.
  -dumpmachine             Display the compiler's target processor.
```
From the output of `ld --help`:
```
  -rpath PATH                 Set runtime shared library search path
  -rpath-link PATH            Set link time shared library search path
  -shared, -Bshareable        Create a shared library
  -pie, --pic-executable      Create a position independent executable
```
From the output of `qemu-system-x86_64 --help`
```
-serial dev     redirect the serial port to char device 'dev'
-parallel dev   redirect the parallel port to char device 'dev'
-monitor dev    redirect the monitor to char device 'dev'
```
### 2. Long option with a single plus (`+`) instead of a double hyphen-minus (`--`)
I'm sure there are others, but I can point to the output of `Xwayland --help` for this:
```
+xinerama              Enable XINERAMA extension
-xinerama              Disable XINERAMA extension
...
+extension name        Enable extension
-extension name        Disable extension
```
### 3. Short option with a plus (`+`) instead of a hyphen-minus (`-`)
In this case it is a short option because it can be combined as `+OO opt1 opt2`. Although the value needs to be separated with a space: `+Oopt1` does not work. From `man 1 bash`:
```
       [-+]O [shopt_option]
                 shopt_option  is  one  of  the  shell options accepted by the
                 shopt  builtin  (see  SHELL  BUILTIN  COMMANDS  below).    If
                 shopt_option is present, -O sets the value of that option; +O
                 unsets it.  If shopt_option is not supplied,  the  names  and
                 values  of the shell options accepted by shopt are printed on
                 the standard output.  If the invocation  option  is  +O,  the
                 output is displayed in a format that may be reused as input.
```
### 4. No space between option and value
As in short options, but the option is not a single character, so combining is not possible. From `gcc --help`:
```
  -Wa,<options>            Pass comma-separated <options> on to the assembler.
  -Wp,<options>            Pass comma-separated <options> on to the preprocessor.
  -Wl,<options>            Pass comma-separated <options> on to the linker.
```
(I guess it looks like `W` is the short option and the first value `a`/`p`/`l` signifies assembler, preprocessor, or linker. But combining such short options would not make sense.)
### 5. Long option with value after equals (`=`), but without the double hyphen-minus (`--`)
From `dd --help`
```
  count=N         copy only N input blocks
  ibs=BYTES       read up to BYTES bytes at a time (default: 512)
  if=FILE         read from FILE instead of stdin
  iflag=FLAGS     read as per the comma separated symbol list
  obs=BYTES       write BYTES bytes at a time (default: 512)
  of=FILE         write to FILE instead of stdout
  oflag=FLAGS     write as per the comma separated symbol list
```
### 6. Windows way: slash (`/`) instead of double hyphen-minus (`--`) and colon (`:`) instead of equals (`=`)
Not very familiar with Windows command line parameters, but from examples it seems only long options, mostly single-letter, are used. They are still "long" options since there is no combining. Option values are separated by a space or a colon (`:`). Also, `/?` is the equivalent of `-h, --help`.
```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<b><f> | <f>}] [/e:{on | off}] [/f:{on | off}] [/v:{on | off}] [<string>]

icacls <filename> [/grant[:r] <sid>:<perm>[...]] [/deny <sid>:<perm>[...]] [/remove[:g|:d]] <sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<policy>[...]]

pathping [/n] [/h <maximumhops>] [/g <hostlist>] [/p <Period>] [/q <numqueries> [/w <timeout>] [/i <IPaddress>] [/4 <IPv4>] [/6 <IPv6>][<targetname>]

winnt32 [/checkupgradeonly] [/cmd: <CommandLine>] [/cmdcons] [/copydir:{i386|ia64}\<FolderName>] [/copysource: <FolderName>] [/debug[<Level>]:[ <FileName>]] [/dudisable] [/duprepare: <pathName>] [/dushare: <pathName>] [/emsport:{com1|com2|usebiossettings|off}] [/emsbaudrate: <BaudRate>] [/m: <FolderName>]  [/makelocalsource] [/noreboot] [/s: <Sourcepath>] [/syspart: <DriveLetter>] [/tempdrive: <DriveLetter>] [/udf: <ID>[,<UDB_File>]] [/unattend[<Num>]:[ <AnswerFile>]]
```

### Describe the solution you'd like

Instead of (or in addition to) specifying short vs. long, maybe we could specify:
- The prefix: `"-"`, `"--"`, `"/"`, `"+"`, `""`
- Whether to combine as in short options or not
- Whether to allow, or require, or disallow space between the option name and its value
- The separator between the option name and its value(s): `"="`, `":"`

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `T: new feature` added by @mkayaalp on 2021-05-05 10:44_

---

_Label `C: parsing` added by @pksunkara on 2021-05-06 11:12_

---

_Label `W: maybe` added by @pksunkara on 2021-05-06 11:12_

---

_Referenced in [clap-rs/clap#1210](../../clap-rs/clap/issues/1210.md) on 2021-11-14 22:26_

---

_Referenced in [clap-rs/clap#815](../../clap-rs/clap/issues/815.md) on 2021-11-15 06:29_

---

_Comment by @epage on 2021-11-15 14:47_

[My comment](https://github.com/clap-rs/clap/issues/1210#issuecomment-956656955) in #1210 speaks of a general solution that would cover the wider case than just single hyphens in long arguments.

> Whether to allow, or require, or disallow space between the option name and its value

We do have [`requires_equals`](https://docs.rs/clap/2.33.3/clap/struct.Arg.html#method.require_equals), granted that only switches between allow space vs disallow and doesn't cover require space.

It doesn't allow a different separator value.  We'd need to create a name, add support for it, and deprecate the existing function (since it name references the syntax and not the general semantics).  We'd end up with a pair like `use_delimiter` and `value_delimiter`.

---

_Referenced in [clap-rs/clap#3026](../../clap-rs/clap/issues/3026.md) on 2021-11-15 14:52_

---

_Comment by @epage on 2021-11-15 14:53_

I broke the name/value separator part out into a child task

---

_Comment by @mkayaalp on 2021-11-15 20:00_

Thank you for looking into this. 

> We do have [`requires_equals`](https://docs.rs/clap/2.33.3/clap/struct.Arg.html#method.require_equals), granted that only switches between allow space vs disallow and doesn't cover require space.

Since creating this issue, I looked at CLI guidelines of POSIX, GNU, Solaris and realized that handling of space in clap for optional option-arguments is not the Unix way: #3030

---

_Referenced in [MarcelRobitaille/last-rs#2](../../MarcelRobitaille/last-rs/issues/2.md) on 2021-11-24 18:28_

---

_Referenced in [epage/clapng#185](../../epage/clapng/issues/185.md) on 2021-12-06 21:14_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:10_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:10_

---

_Label `S-waiting-on-decision` removed by @epage on 2021-12-13 16:38_

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-13 16:38_

---

_Referenced in [clap-rs/clap#3635](../../clap-rs/clap/pulls/3635.md) on 2022-04-15 17:59_

---

_Comment by @Burning1020 on 2022-11-29 12:25_

Excuse me, when will we support for the single hyphen feature? Is there any PR working on it?

---

_Comment by @epage on 2022-11-29 12:41_

Work to be done for this:
1. ~~Implementing `clap_lex`~~ Done
2. Creating a trait for it that is object safe (not started)
3. Modify `clap::Command` so it accepts an alternative `clap_lex` implementation

When doing these, we need to balance
- Performance
- Compile time and binary size for the default case
- API compatibility
  - the trait needs to be defined somewhere that has the same API guarantees as `clap`
  - `clap_lex` does not yet have those guarantees.  I would rather not have `os_str_bytes` in the API either

However, this is not a high priority for me but the [4.x issues](https://github.com/clap-rs/clap/issues?q=is%3Aopen+is%3Aissue+milestone%3A4.x) are.

---

_Referenced in [containerd/rust-extensions#120](../../containerd/rust-extensions/issues/120.md) on 2023-02-16 06:54_

---

_Referenced in [clap-rs/clap#4948](../../clap-rs/clap/issues/4948.md) on 2023-06-05 14:42_

---

_Referenced in [uutils/coreutils#4950](../../uutils/coreutils/pulls/4950.md) on 2023-06-23 21:16_

---

_Referenced in [microsoft/pdblister#5](../../microsoft/pdblister/issues/5.md) on 2023-12-15 18:47_

---

_Referenced in [clap-rs/clap#5377](../../clap-rs/clap/issues/5377.md) on 2024-02-26 15:32_

---

_Referenced in [uutils/coreutils#5966](../../uutils/coreutils/pulls/5966.md) on 2024-03-13 12:20_

---

_Referenced in [clap-rs/clap#5742](../../clap-rs/clap/issues/5742.md) on 2024-09-20 19:17_

---

_Referenced in [clap-rs/clap#6082](../../clap-rs/clap/issues/6082.md) on 2025-08-05 18:21_

---
