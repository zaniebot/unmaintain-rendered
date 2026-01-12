```yaml
number: 2126
title: "No match with literal string `<bindingRedirect oldVersion=\"0`."
type: issue
state: closed
author: dotjpg3141
labels: []
assignees: []
created_at: 2022-01-14T09:17:55Z
updated_at: 2022-01-14T12:39:26Z
url: https://github.com/BurntSushi/ripgrep/issues/2126
synced_at: 2026-01-12T16:13:24Z
```

# No match with literal string `<bindingRedirect oldVersion="0`.

---

_@dotjpg3141_

#### What version of ripgrep are you using?

ripgrep 13.0.0 (rev af6b6c543b)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Via chocolatey

#### What operating system are you using ripgrep on?

Windows 10

#### Describe your bug.

Ripgrep doesn't find a match with literal string `<bindingRedirect oldVersion="0`.
I hope this is not a quote/escaping/whitespace issue on my side. See also the repro script below.
Removing the trailing `0` reports a single match as expected.

#### What are the steps to reproduce the behavior?

**app.config**
```xml
<bindingRedirect oldVersion="0.0.0.0-12.0.0.0" newVersion="12.0.0.0" />
```

**Input (Powershell)**
```powershell
rg -F "<bindingRedirect oldVersion=`"0"
```

**NOTE:** `" escapes quotation marks.

#### What is the actual behavior?

No output is provided.

#### What is the expected behavior?

A single match result.

#### Repro Script
[repro.zip](https://github.com/BurntSushi/ripgrep/files/7869218/repro.zip)

Powershell script:
```powershell
Write-Host "============= Start ============="
$x = "<bindingRedirect oldVersion=`"";
Write-Host "||$x||"
rg -F --no-config --debug $x
Write-Host "============= End ============="

Write-Host

Write-Host "============= Start ============="
$y = "<bindingRedirect oldVersion=`"0";
Write-Host "||$y||"
rg -F --no-config --debug $y
Write-Host "============= End ============="

Write-Host "Done."
```

Output:
<details>
  <summary>Click to expand!</summary>

```
============= Start =============
||<bindingRedirect oldVersion="||
DEBUG|rg::args|crates/core\args.rs:527: not reading config files because --no-config is present
DEBUG|grep_regex::literal|crates\regex\src\literal.rs:58: literal prefixes detected: Literals { lits: [Complete(<bindingRedirect oldVersion=")], limit_size: 250, limit_class: 10 }
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
app.config
1:<bindingRedirect oldVersion="0.0.0.0-12.0.0.0" newVersion="12.0.0.0" />
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
============= End =============

============= Start =============
||<bindingRedirect oldVersion="0||
DEBUG|rg::args|crates/core\args.rs:527: not reading config files because --no-config is present
DEBUG|grep_regex::literal|crates\regex\src\literal.rs:58: literal prefixes detected: Literals { lits: [Complete(<bindingRedirect oldVersion=0)], limit_size: 250, limit_class: 10 }
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
============= End =============
Done.
```
</details>


---

_Comment by @dotjpg3141 on 2022-01-14 09:33_

I *think* the escaped quote is correct in PowerShell:

```powershell
PS> Write-Host "<bindingRedirect oldVersion=`""
<bindingRedirect oldVersion="
```

But it disappears in the debug log:

```
DEBUG|grep_regex::literal|crates\regex\src\literal.rs:58: literal prefixes detected: Literals { lits: [Complete(<bindingRedirect oldVersion=0)], limit_size: 250, limit_class: 10 }
```

Edit: Not wrongly escaped:

```rust
fn main() {
    for arg in std::env::args().skip(1) {
        println!("{arg}");
    }
}
```

```powershell
PS> cargo run --quiet "<bindingRedirect oldVersion=`""
<bindingRedirect oldVersion="
```

Edit2: Using a regex with `\x22` as quote works fine.

---

_Comment by @BurntSushi on 2022-01-14 11:54_

The output you included shows a match? So I'm not quite sure what is wrong about it?

(I don't use Windows and I know very little about PowerShell, so I'm afraid I can't be of much help here. But if there is a problem here, it's almost certainly not worth ripgrep.)

---

_Comment by @dotjpg3141 on 2022-01-14 12:39_

Okay. This is apparently a weirdness in PowerShell (executed with the rust program above):

```
PS> $x = "aa`"bb"
PS> Write-Host $x
aa"bb
PS> cargo run --quiet $x
aabb
```

---

_Closed by @dotjpg3141 on 2022-01-14 12:39_

---
