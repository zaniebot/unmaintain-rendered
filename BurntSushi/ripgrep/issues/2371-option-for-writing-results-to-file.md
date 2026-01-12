```yaml
number: 2371
title: Option for writing results to file
type: issue
state: closed
author: Simran-B
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2022-12-15T23:38:02Z
updated_at: 2023-11-24T18:54:36Z
url: https://github.com/BurntSushi/ripgrep/issues/2371
synced_at: 2026-01-12T16:13:24Z
```

# Option for writing results to file

---

_@Simran-B_

#### Describe your feature request

An option `--output` or `--outfile` to write what ripgrep would print in a terminal to a file instead but byte-exact.

Usually, you would pipe the stdout to a file like `rg "expr" infile.txt > outfile.txt` to do this. However, there are circumstances where this doesn't work as expected:

- Line breaks may not be preserved by the terminal as-is.
  Both cmd.exe (using conhost.exe) and PowerShell (pwsh using the Terminal app) on Windows turn `\r` and `\n` into `\r\n`. A sequence of `\n\r` ends up as `\r\n\r\n`. Furthermore, a trailing newline (`\r\n`) is appended.
  
  Git Bash from Git for Windows leaves the line breaks as they are but appends a trailing `\n`.

  `\r` makes the terminals on Windows write over the current line, but it's difficult to understand what's going on because redirecting the output to a file does not give you the exact byte stream either.

- Special terminal characters can mess with the redirection
  https://github.com/BurntSushi/ripgrep/issues/1309#issuecomment-633927149

- Spaces after the command might end up in the output (although this appears to be limited to spaces before `|` and cmd.exe) 
  https://github.com/BurntSushi/ripgrep/issues/1500

---

_Comment by @BurntSushi on 2022-12-16 00:30_

I suppose an `--outfile` would be a fine thing to add. It solves a real problem, although I note that it sounds like it papers over some rather subtle issues in various Windows environments. One wonders how one is supposed to know to use `--outfile` when output redirection exists. But for those that do know, it seems useful and should have low implementation cost.

I'm not sure what the flag name should be. I am more partial to `--outpath` personally. I think `--output` is perhaps a bit too vague and `--outfile` is not quite as precise as `--outpath`.

---

_Label `enhancement` added by @BurntSushi on 2022-12-16 00:30_

---

_Label `help wanted` added by @BurntSushi on 2022-12-16 00:30_

---

_Comment by @Simran-B on 2022-12-16 22:06_

Yes, it's more of a workaround for a problem outside ripgrep's control. I would appreciate it, nonetheless.

I think it's sufficient to give the option an intuitive name so that people looking for this kind of functionality can easily find it, accompanied by a `--help` text explaining the reason for this option and that you may use redirection instead (with caveats).

The only existing options to specify file(path)s seem to be:

- `-f, --file <PATTERNFILE>...`
- `--ignore-file <PATH>...`

`--output-file <PATH>` would fit well into this naming scheme. `-O` would be free as shorthand, but it might be better to reserve it for a more commonly used future option.

---

_Comment by @dibrinsofor on 2023-10-05 13:06_

was this ever worked on?


---

_Comment by @BurntSushi on 2023-10-05 13:43_

No. If it was, you'd very likely see an update of some kind in this issue.

---

_Comment by @BurntSushi on 2023-11-24 18:54_

I looked into this and I don't understand why this option is needed. The linked issues don't appear to be related to this. And the OP here describes problems with writing to a Windows terminal. But redirecting stdout means that ripgrep is no longer writing to a terminal.

I think the biggest problem with the way this feature request is that there are zero instructions for reproducing a real problem. Imagine, for example, that I add an `--outfile` flag. How do I test that it solves the problem here?

If someone wants to write up more detail with a real reproduction that I can follow, then I might be willing to re-open this and add support for a `--outfile` flag. But I'm going to pass for now because it seems like an unnecessary complication.

---

_Closed by @BurntSushi on 2023-11-24 18:54_

---
