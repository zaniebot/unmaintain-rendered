```yaml
number: 1622
title: "-z option on Windows requires \"gzip\" no documented dependencies and no Errors in execution"
type: issue
state: closed
author: cwitter
labels:
  - doc
assignees: []
created_at: 2020-06-19T13:59:10Z
updated_at: 2023-11-25T20:04:02Z
url: https://github.com/BurntSushi/ripgrep/issues/1622
synced_at: 2026-01-12T16:13:23Z
```

# -z option on Windows requires "gzip" no documented dependencies and no Errors in execution

---

_@cwitter_

#### What version of ripgrep are you using?

ripgrep 12.1.1 (rev 7cb211378a)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

https://github.com/BurntSushi/ripgrep/releases/download/12.1.1/ripgrep-12.1.1-x86_64-pc-windows-msvc.zip

#### What operating system are you using ripgrep on?

Windows 10 Professional

#### Describe your bug.

Using ripgrep with the "-z" option and attempting to search GZip files it reports no errors when missing the "gzip" dependency.

#### What are the steps to reproduce the behavior?

rg -z -i info -g *

#### What is the actual behavior?

rg -z -i info -g * --debug

```
|crates\cli\src\decompress.rs:196: 2020-06-17-1.log.gz: error spawning command '"gzip" "-d" "-c" "2020-06-17-1.log.gz"': The system cannot find the file specified. (os error 2) (falling back to uncompressed reader)
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:196: 2020-06-18-1.log.gz: error spawning command '"gzip" "-d" "-c" "2020-06-18-1.log.gz"': The system cannot find the file specified. (os error 2) (falling back to uncompressed reader)
```

#### What is the expected behavior?
No dependencies are listed for the Windows installation, expected behavior is it would work as expected or provide an error calling out the missing dependencies. Minor documentation updates to call out the requirements on Windows might help others as well.

What do you think ripgrep should have done?
Include a decompression error similar to the one in the "debug" output to inform the user of the failure.
With GNU Gzip installed and in the PATH everything works as expected for GZ files.

---

_Comment by @BurntSushi on 2020-06-19 14:10_

I'm happy to have the docs for `-z` improved to explain this behavior, and we can leave this ticket open for that.

But no, the error will stay in the `--debug` output and will not be displayed by default. Otherwise, ripgrep will become quite noisy in certain cases.

---

_Label `doc` added by @BurntSushi on 2020-06-19 14:10_

---

_Comment by @golimarrrr on 2021-10-07 08:46_

It should probably say: "This option expects the decompression binaries to be available in your PATH. If they are not found, no error will be echoed"
I have gzip installed and thought it was in the PATH, and read that ripgrep supports searching into gz files, so I thought that gzip was being used and the pattern was not present in the file, but it was

---

_Closed by @BurntSushi on 2023-11-25 20:04_

---
