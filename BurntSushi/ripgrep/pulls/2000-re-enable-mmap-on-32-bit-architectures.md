```yaml
number: 2000
title: Re-enable mmap on 32-bit architectures
type: pull_request
state: merged
author: adamreichold
labels: []
assignees: []
merged: true
base: master
head: memmap-32-bits
created_at: 2021-09-23T07:52:45Z
updated_at: 2023-05-19T12:27:38Z
url: https://github.com/BurntSushi/ripgrep/pull/2000
synced_at: 2026-01-12T18:23:14Z
```

# Re-enable mmap on 32-bit architectures

---

_@adamreichold_

memmap2 v0.3.0 introduced a [regression](https://github.com/RazrFalcon/memmap2-rs/commit/5e271224c8411c89b42060294f9393cfc7b12a2a) when trying to map files larger than 4GB on 32-bit architectures which was subsequently [fixed](https://github.com/RazrFalcon/memmap2-rs/commit/9aa838aed99a4879d8357ff295a0ca1c98ba1ae5) in v0.3.1.

This commit bumps locked version of the memmap2 dependency to the current v0.5.0 and reverts fdfc418be55ff91e0c2efad6a3e27db054cb5534 to re-enable mmap on 32-bit architectures as a different approach to fixing #1911.

This was tested to report matches from the end of a 5GB file using MinGW and Wine:
```
~/.wine/drive_c> wine rg.exe 3z5llj3n8b56dcpoj5aj4rlmq3bdpie wikidatawiki-20210901-pages-articles1.xml-p1p441397 --debug
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(3z5llj3n8b56dcpoj5aj4rlmq3bdpie)], limit_size: 250, limit_class: 10 }
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_searcher::searcher::mmap|crates/searcher/src/searcher/mmap.rs:86: wikidatawiki-20210901-pages-articles1.xml-p1p441397: failed to open memory map: memory map length overflows usize
1727003:      <sha1>3z5llj3n8b56dcpoj5aj4rlmq3bdpie</sha1>
```
whereas still using memmap2 v0.3.0 but fdfc418be55ff91e0c2efad6a3e27db054cb5534 reverted would silently fail
``` 
~/.wine/drive_c> wine rg.exe 3z5llj3n8b56dcpoj5aj4rlmq3bdpie wikidatawiki-20210901-pages-articles1.xml-p1p441397 --debug
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(3z5llj3n8b56dcpoj5aj4rlmq3bdpie)], limit_size: 250, limit_class: 10 }
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates/cli/src/decompress.rs:482: uncompress: could not find executable in PATH
```

---

_Comment by @adamreichold on 2023-05-19 12:09_

I would be grateful for a short feedback whether this is wanted at all.

I think the root cause for the issues is highly likely to have been the bug in memmap2. Since that dependency has been bumped in the meantime, I don't think the workaround needs to be kept around.

---

_Comment by @BurntSushi on 2023-05-19 12:22_

I think I buy it. Your research is enough to give me some confidence that this won't re-occur. If it does, we can re-apply the patch.

---

_Merged by @BurntSushi on 2023-05-19 12:23_

---

_Closed by @BurntSushi on 2023-05-19 12:23_

---

_Branch deleted on 2023-05-19 12:27_

---
