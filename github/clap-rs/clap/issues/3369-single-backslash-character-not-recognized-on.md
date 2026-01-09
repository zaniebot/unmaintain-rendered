---
number: 3369
title: Single backslash character not recognized on Windows
type: issue
state: closed
author: jgranduel
labels:
  - C-bug
assignees: []
created_at: 2022-01-29T14:43:29Z
updated_at: 2022-01-31T08:15:21Z
url: https://github.com/clap-rs/clap/issues/3369
synced_at: 2026-01-07T13:12:19-06:00
---

# Single backslash character not recognized on Windows

---

_Issue opened by @jgranduel on 2022-01-29 14:43_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.58

### Clap Version

clap v3.0.13

### Minimal reproducible code

Hi, 

I was trying [csv2parquet](https://github.com/domoritz/csv2parquet) on Windows. csv2parquet parses CSV files and I happened to have TSV file. I couldn't get it working although it has a `--delimiter` option. The author told me to simply use `\t` for indicating tab separator, but it doesn't work on Windows (Windows 10/PowerShell 7.2, but not relevant AFAIK). We think it's a `clap` issue.
Here the link to the [csv2parquet #58 ](https://github.com/domoritz/csv2parquet/issues/58) issue I posted.

In Rust code, the deliimiter is a simple char:
```
delimiter: char, 
```
How to use `\t` (or other escaped characters) on Windows or any ASCII/UNICODE field, line separtors?



### Steps to reproduce the bug with the above code

csv2parquet.exe -d \t a.csv a.parquet

### Actual Behaviour

```
csv2parquet.exe -d \t a.csv a.parquet
error: Invalid value for '--delimiter <DELIMITER>': too many characters in string
```

### Expected Behaviour

`\t` recognized as "," on Windows, or any ASCII/UNICODE separator like U001C file separator, U+001D group separator, U+001E record separator, U+001F unit separator, U+2028   line separator, U+2029   paragraph separator.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @jgranduel on 2022-01-29 14:43_

---

_Referenced in [domoritz/csv2parquet#58](../../domoritz/csv2parquet/issues/58.md) on 2022-01-29 14:49_

---

_Comment by @epage on 2022-01-29 19:53_

Can you include the debug output requested in the template?  That will show how clap is parsing things.

As for how its being handled, thats all in the CLI parser which, for windows, is baked into Rust.  A combination of things to try
- Quoting
- Using the `-d=` syntax

---

_Comment by @jgranduel on 2022-01-30 11:18_

Mentionned in [csv2parquet #58](https://github.com/domoritz/csv2parquet/issues/58)
Thanks for your answer.  It might have been a Windows issue tab character is **`t** in PowerShell and I didn't use it properly.
So these 3 commands worked out:

```
>csv2parquet.exe -d "`t" a.tsv a.parquet     # OK
>csv2parquet.exe "-d=`t" a.tsv aa.parquet  # OK 
 
>csv2parquet.exe -d `t a.tsv aaa.parquet  # OK, unquoted, space is needed!
```

This unquoted syntax 
```
csv2parquet.exe -d=`t a.tsv a.parquet  # KO
```
failed.


---

_Comment by @jgranduel on 2022-01-31 08:15_

As it seems to be a Windows feature, I'm closing this issue. Thanks.

---

_Closed by @jgranduel on 2022-01-31 08:15_

---
