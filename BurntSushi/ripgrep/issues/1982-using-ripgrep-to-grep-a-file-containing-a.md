```yaml
number: 1982
title: Using ripgrep to grep a file containing a particular spelling with a particular regular expression resulted in an unusual PCRE2 error.
type: issue
state: closed
author: ashidaharo
labels:
  - bug
assignees: []
created_at: 2021-09-02T08:28:56Z
updated_at: 2023-11-24T20:16:51Z
url: https://github.com/BurntSushi/ripgrep/issues/1982
synced_at: 2026-01-12T16:13:24Z
```

# Using ripgrep to grep a file containing a particular spelling with a particular regular expression resulted in an unusual PCRE2 error.

---

_@ashidaharo_

#### What version of ripgrep are you using?

The version used for Current VS Code(ripgrep 13.0.0 (rev af6b6c543b))

#### How did you install ripgrep?

Installed by VS Code

#### What operating system are you using ripgrep on?

Windows 10 Home(19043.1200)

#### Describe your bug.

An unusual PCRE2 error occurred when trying to find a file containing a particular magic spell in VS Code using a particular regular expression.
I am not familiar with the internal workings of ripgrep or the specifications of PCRE2, so I am unable to determine whether this problem is caused by PCRE2 or the contact surface between ripgrep and PCRE2 (or the way VS Code calls ripgrep).
So for the time being, I will report the bug to this ripgrep, which is the executable file that spit out the error.

#### What are the steps to reproduce the behavior?

Prepare a file containing the string to be searched (here assumed to be "foo") and the following lines.
```
AbcdefghijklmnopqrstuvwxyzAbcdefghijklmnopqrstuvwxyzAbcdefghijklmn
-あいうえおかきくけこさしすせそたちつてと
AbcdefghijklmnopqrstuvwxyzAbcdefghijklmnopqrstuvwxyzAbcdefghijklmn
```
Use "(?<!\w) foo" as a regular expression.
Here is how VSCode called ripgrep.. Please replace the folder appropriately.
```
d:\home\tool\VSCode\resources\app\node_modules.asar.unpacked\vscode-ripgrep\bin\rg.exe --hidden --ignore-case -g '!**/.git' -g '!**/.svn' -g '!**/.hg' -g '!**/CVS' -g '!**/.DS_Store' -g '!**/node_modules' -g '!**/bower_components' -g '!**/*.code-search' --max-filesize '17179869184' --no-ignore-parent --follow --crlf --auto-hybrid-regex --regexp '(?<!\w)foo' --no-config --no-ignore-global --json --multiline -- '.'
 - cwd: d:\test
```
Magic spells can be freely replaced within the ASCII range for alphabets, and at least within the hiragana, katakana, and kanji ranges for Japanese characters.

#### What is the actual behavior?
```
PS D:\test> d:\home\tool\VSCode\resources\app\node_modules.asar.unpacked\vscode-ripgrep\bin\rg.exe --hidden --ignore-case -g '!**/.git' -g '!**/.svn' -g '!**/.hg' -g '!**/CVS' -g '!**/.DS_Store' -g '!**/node_modules' -g '!**/bower_components' -g '!**/*.code-search' --max-filesize '17179869184' --no-ignore-parent --follow --debug --crlf --auto-hybrid-regex --regexp '(?<!\w)f' --no-config --no-ignore-global --json --multiline -- '.' d:\test
DEBUG|rg::args|crates/core\args.rs:527: not reading config files because --no-config is present
DEBUG|rg::args|crates/core\args.rs:626: error building Rust regex in hybrid mode:
regex parse error:
    (?<!\w)f
    ^^^^
error: look-around, including look-ahead and look-behind, is not supported
DEBUG|globset|crates\globset\src\lib.rs:421: built glob set; 0 literals, 7 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
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
d:\test\baz.txt: PCRE2: error matching: UTF-8 error: 1 byte missing at end
.\baz.txt: PCRE2: error matching: UTF-8 error: 1 byte missing at end
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
{"data":{"elapsed_total":{"human":"0.348348s","nanos":348347700,"secs":0},"stats":{"bytes_printed":0,"bytes_searched":0,"elapsed":{"human":"0.000000s","nanos":0,"secs":0},"matched_lines":0,"matches":0,"searches":0,"searches_with_match":0}},"type":"summary"}
```

#### What is the expected behavior?

The error "UTF-8 error: Missing 1 byte at the end" is obviously not a proper error, since the magic spell itself is a legitimate Unicode string.


---

_Comment by @BurntSushi on 2021-09-02 11:07_

Given your instructions, I can't reproduce the issue:

```
$ cat haystack
AbcdefghijklmnopqrstuvwxyzAbcdefghijklmnopqrstuvwxyzAbcdefghijklmn
-あいうえおかきくけこさしすせそたちつてと
AbcdefghijklmnopqrstuvwxyzAbcdefghijklmnopqrstuvwxyzAbcdefghijklmn
foo

$ rg '(?<!\w)foo' --auto-hybrid-regex --crlf --no-config --json --multiline
{"type":"begin","data":{"path":{"text":"haystack"}}}
{"type":"match","data":{"path":{"text":"haystack"},"lines":{"text":"foo\n"},"line_number":4,"absolute_offset":196,"submatches":[{"match":{"text":"foo"},"start":0,"end":3}]}}
{"type":"end","data":{"path":{"text":"haystack"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":46340,"human":"0.000046s"},"searches":1,"searches_with_match":1,"bytes_searched":200,"bytes_printed":227,"matched_lines":1,"matches":1}}}
{"data":{"elapsed_total":{"human":"0.003138s","nanos":3137698,"secs":0},"stats":{"bytes_printed":227,"bytes_searched":200,"elapsed":{"human":"0.000046s","nanos":46340,"secs":0},"matched_lines":1,"matches":1,"searches":1,"searches_with_match":1}},"type":"summary"}
```

With that said, I'm not particularly confident about your reproduction steps. Clearly, you are searching more than just the errant file here, so I'd request that you please start widdling this down, ideally to a directory containing the single file that is causing problems.

Could you provide the _exact_ contents of the file tripping the error? Namely, `d:\test\baz.txt`. If it's too big, then I'd request that you try to decrease its size such that the error still occurs. In addition to copy and pasting the output, please also provide the output of `xxd d:\test\baz.txt` so that we make sure we get the exact byte-for-byte contents. Otherwise, your OS (or something) might replace invalid UTF-8 with something else when copy and pasting.

---

_Comment by @BurntSushi on 2021-09-02 11:08_

It would also be nice if you could decrease the size of the `rg` command itself. There is probably a lot in there that is irrelevant.

---

_Comment by @ashidaharo on 2021-09-02 12:04_

Based on your failure to reproduce, I experimented in my environment, and I found that I lacked the conditions for reproduction.
This magic spell apparently needs to be present "below" the string you want to search for.
Here are the two results of my experiments with the rg command in a lighter weight version based on your reproduction.
```
PS D:\test> cat baz.txt
foo
AbcdefghijklmnopqrstuvwxyzAbcdefghijklmnopqrstuvwxyzAbcdefghijklmn
-あいうえおかきくけこさしすせそたちつてと
AbcdefghijklmnopqrstuvwxyzAbcdefghijklmnopqrstuvwxyzAbcdefghijklmn

PS D:\test> d:\home\tool\VSCode\resources\app\node_modules.asar.unpacked\vscode-ripgrep\bin\rg.exe '(?<!\w)foo' --auto-hybrid-regex --crlf --no-config --json --multiline
baz.txt: PCRE2: error matching: UTF-8 error: 1 byte missing at end
{"data":{"elapsed_total":{"human":"0.271541s","nanos":271540700,"secs":0},"stats":{"bytes_printed":0,"bytes_searched":0,"elapsed":{"human":"0.000000s","nanos":0,"secs":0},"matched_lines":0,"matches":0,"searches":0,"searches_with_match":0}},"type":"summary"}
```
```
PS D:\test> cat baz.txt
AbcdefghijklmnopqrstuvwxyzAbcdefghijklmnopqrstuvwxyzAbcdefghijklmn
-あいうえおかきくけこさしすせそたちつてと
AbcdefghijklmnopqrstuvwxyzAbcdefghijklmnopqrstuvwxyzAbcdefghijklmn
foo

PS D:\test> d:\home\tool\VSCode\resources\app\node_modules.asar.unpacked\vscode-ripgrep\bin\rg.exe '(?<!\w)foo' --auto-hybrid-regex --crlf --no-config --json --multiline
{"type":"begin","data":{"path":{"text":"baz.txt"}}}
{"type":"match","data":{"path":{"text":"baz.txt"},"lines":{"text":"foo\r\n"},"line_number":4,"absolute_offset":199,"submatches":[{"match":{"text":"foo"},"start":0,"end":3}]}}
{"type":"end","data":{"path":{"text":"baz.txt"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":57300,"human":"0.000057s"},"searches":1,"searches_with_match":1,"bytes_searched":204,"bytes_printed":227,"matched_lines":1,"matches":1}}}
{"data":{"elapsed_total":{"human":"0.267311s","nanos":267311000,"secs":0},"stats":{"bytes_printed":227,"bytes_searched":204,"elapsed":{"human":"0.000057s","nanos":57300,"secs":0},"matched_lines":1,"matches":1,"searches":1,"searches_with_match":1}},"type":"summary"}
```

---

_Comment by @BurntSushi on 2021-09-02 12:11_

That doesn't reproduce for me either unfortunately. The problem is that the error is reporting invalid UTF-8. Because of that, I really need to be _absolutely sure_ that I have precisely the same contents of `baz.txt` as you. That's why I asked for the output of `xxd baz.txt`. An alternative would be to upload that file somewhere (to GitHub should be fine?) so that I can download it as-is.

---

_Comment by @BurntSushi on 2021-09-02 12:12_

Also, could you check whether the same problem happens with the Windows MSVC binary from ripgrep's release? https://github.com/BurntSushi/ripgrep/releases/tag/13.0.0

---

_Comment by @ashidaharo on 2021-09-02 12:14_

Oh my god, I forgot to check the conditions that I should have experimented with the most, forgetting that I was in a Windows environment! My sincere apologies!
It was a file saved in CRLF!
It didn't happen when the file was saved in LF! How could I have been so stupid?

---

_Comment by @ashidaharo on 2021-09-02 12:27_

I used ripgrep-13.0.0-x86_64-pc-windows-msvc.zip.
Also, the Powershell environment I'm using is old and cannot display UTF-8 files without BOM in the correct encoding, so I'm experimenting here using UTF-8 files saved with BOM, but I've confirmed that the problem is also reproduced when saved in UTF-8 without BOM.
```
PS D:\test> d:\rg.exe --version
ripgrep 13.0.0 (rev af6b6c543b)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

PS D:\test> cat baz.txt        
foo
AbcdefghijklmnopqrstuvwxyzAbcdefghijklmnopqrstuvwxyzAbcdefghijklmn
-あいうえおかきくけこさしすせそたちつてと
AbcdefghijklmnopqrstuvwxyzAbcdefghijklmnopqrstuvwxyzAbcdefghijklmn

PS D:\test> d:\rg.exe '(?<!\w)foo' --auto-hybrid-regex --crlf --no-config --json --multiline
baz.txt: PCRE2: error matching: UTF-8 error: 1 byte missing at end
{"data":{"elapsed_total":{"human":"0.264899s","nanos":264898600,"secs":0},"stats":{"bytes_printed":0,"bytes_searched":0,"elapsed":{"human":"0.000000s","nanos":0,"secs":0},"matched_lines":0,"matches":0,"searches":0,"searches_with_match":0}},"type":"summary"}
```

---

_Comment by @ashidaharo on 2021-09-02 12:43_

https://raw.githubusercontent.com/ashidaharo/ForReport/main/test/baz.txt
Uploaded.
I set core.autocrlf to false in the git configuration, so the newline code is still there, and I also re-downloaded it myself and confirmed that the same error occurs.

---

_Comment by @ashidaharo on 2021-09-02 14:32_

It turned out to be the same old Windows silliness.
The problem is apparently only caused by the Windows version of the executable or the Windows mechanism.
I did a comparison experiment in three environments.
One is ripgrep for Linux installed on a WSL2 virtual machine with Ubuntu 18.04 environment.
The next one was installed on Windows from the ripgrep-13.0.0-x86_64-pc-windows-msvc.zip file that I used earlier.
Finally, I installed it on Windows from ripgrep-13.0.0-x86_64-pc-windows-gnu.zip, which I prepared additionally just for fun.
The result is shown here.
```
ashida@DESKTOP-MTMA9GA:/mnt/d/test$ rg '(?<!\w)foo' --auto-hybrid-regex --crlf --no-config --json --multiline
{"type":"begin","data":{"path":{"text":"baz.txt"}}}
{"type":"match","data":{"path":{"text":"baz.txt"},"lines":{"text":"foo\r\n"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":"foo"},"start":0,"end":3}]}}
{"type":"end","data":{"path":{"text":"baz.txt"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":472300,"human":"0.000472s"},"searches":1,"searches_with_match":1,"bytes_searched":204,"bytes_printed":225,"matched_lines":1,"matches":1}}}
{"data":{"elapsed_total":{"human":"0.009854s","nanos":9854200,"secs":0},"stats":{"bytes_printed":225,"bytes_searched":204,"elapsed":{"human":"0.000472s","nanos":472300,"secs":0},"matched_lines":1,"matches":1,"searches":1,"searches_with_match":1}},"type":"summary"}
```
```
ashida@DESKTOP-MTMA9GA:/mnt/d/test$ /mnt/d/rg_gnu.exe '(?<!\w)foo' --auto-hybrid-regex --crlf --no-config --json --multi
line
baz.txt: PCRE2: error matching: UTF-8 error: 1 byte missing at end
{"data":{"elapsed_total":{"human":"0.178582s","nanos":178581900,"secs":0},"stats":{"bytes_printed":0,"bytes_searched":0,
"elapsed":{"human":"0.000000s","nanos":0,"secs":0},"matched_lines":0,"matches":0,"searches":0,"searches_with_match":0}},
"type":"summary"}
```
```
ashida@DESKTOP-MTMA9GA:/mnt/d/test$ /mnt/d/rg_msvc.exe '(?<!\w)foo' --auto-hybrid-regex --crlf --no-config --json --mult
iline
baz.txt: PCRE2: error matching: UTF-8 error: 1 byte missing at end
{"data":{"elapsed_total":{"human":"0.180438s","nanos":180437800,"secs":0},"stats":{"bytes_printed":0,"bytes_searched":0,
"elapsed":{"human":"0.000000s","nanos":0,"secs":0},"matched_lines":0,"matches":0,"searches":0,"searches_with_match":0}},
"type":"summary"}
```
Note:This comparison experiment had failed.The Linux version binary was mistakenly DL'd as an older version.

---

_Comment by @ashidaharo on 2021-09-02 14:55_

I also discovered another important result of the experiment.
Removed multiline option.
```
ashida@DESKTOP-MTMA9GA:/mnt/d/test$ /mnt/d/rg_msvc.exe '(?<!\w)foo' --auto-hybrid-regex --crlf --no-config --json
{"type":"begin","data":{"path":{"text":"baz.txt"}}}
{"type":"match","data":{"path":{"text":"baz.txt"},"lines":{"text":"foo\r\n"},"line_number":1,"absolute_offset":0,"submat
ches":[{"match":{"text":"foo"},"start":0,"end":3}]}}
{"type":"end","data":{"path":{"text":"baz.txt"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":100400,"human"
:"0.000100s"},"searches":1,"searches_with_match":1,"bytes_searched":204,"bytes_printed":225,"matched_lines":1,"matches":
1}}}
{"data":{"elapsed_total":{"human":"0.186840s","nanos":186839800,"secs":0},"stats":{"bytes_printed":225,"bytes_searched":
204,"elapsed":{"human":"0.000100s","nanos":100400,"secs":0},"matched_lines":1,"matches":1,"searches":1,"searches_with_ma
tch":1}},"type":"summary"}
```


---

_Comment by @ashidaharo on 2021-09-02 15:33_

> That doesn't reproduce for me either unfortunately. The problem is that the error is reporting invalid UTF-8. Because of that, I really need to be _absolutely sure_ that I have precisely the same contents of `baz.txt` as you. That's why I asked for the output of `xxd baz.txt`. An alternative would be to upload that file somewhere (to GitHub should be fine?) so that I can download it as-is.

Uh...I'm very sorry. I had skipped reading this comment.
Is this what you meant?
```
ashida@DESKTOP-MTMA9GA:~/test_rg$ xxd  /mnt/d/test/baz.txt
00000000: 666f 6f0d 0a41 6263 6465 6667 6869 6a6b  foo..Abcdefghijk
00000010: 6c6d 6e6f 7071 7273 7475 7677 7879 7a41  lmnopqrstuvwxyzA
00000020: 6263 6465 6667 6869 6a6b 6c6d 6e6f 7071  bcdefghijklmnopq
00000030: 7273 7475 7677 7879 7a41 6263 6465 6667  rstuvwxyzAbcdefg
00000040: 6869 6a6b 6c6d 6e0d 0a2d e381 82e3 8184  hijklmn..-......
00000050: e381 86e3 8188 e381 8ae3 818b e381 8de3  ................
00000060: 818f e381 91e3 8193 e381 95e3 8197 e381  ................
00000070: 99e3 819b e381 9de3 819f e381 a1e3 81a4  ................
00000080: e381 a6e3 81a8 0d0a 4162 6364 6566 6768  ........Abcdefgh
00000090: 696a 6b6c 6d6e 6f70 7172 7374 7576 7778  ijklmnopqrstuvwx
000000a0: 797a 4162 6364 6566 6768 696a 6b6c 6d6e  yzAbcdefghijklmn
000000b0: 6f70 7172 7374 7576 7778 797a 4162 6364  opqrstuvwxyzAbcd
000000c0: 6566 6768 696a 6b6c 6d6e 0d0a            efghijklmn..
```

---

_Comment by @BurntSushi on 2021-09-02 15:40_

I am actually able to reproduce this on my Linux system. I've made it a little smaller, but not by much.

```
$ cat smaller
foo
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
-あいうえおかきくけこさしすせそたちつてと

$ rg '(?<!\w)foo' -U --engine auto smaller
smaller: PCRE2: error matching: UTF-8 error: 1 byte missing at end
```

But now, this is interesting. If I use `-P` (forces PCRE2) instead of `--engine auto` (which is equivalent to `--auto-hybrid-regex`), then things work:

```
$ rg '(?<!\w)foo' -U -P smaller
1:foo
```

Now, other than trying to detect whether the regex will work with the default engine, I would generally expect these commands to behave the same. But `--trace` reveals otherwise:

```
$ rg '(?<!\w)foo' -U --engine auto smaller --trace
DEBUG|rg::config|crates/core/config.rs:40: /home/andrew/.ripgreprc: arguments loaded from config file: ["--max-columns-preview", "--colors=match:bg:0xff,0x7f,0x00", "--colors=match:fg:white", "--colors=line:none", "--colors=line:fg:magenta", "--colors=path:fg:green", "--type-add=got:*_test.go"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--max-columns-preview", "--colors=match:bg:0xff,0x7f,0x00", "--colors=match:fg:white", "--colors=line:none", "--colors=line:fg:magenta", "--colors=path:fg:green", "--type-add=got:*_test.go", "(?<!\\w)foo", "-U", "--engine", "auto", "smaller", "--trace"]
DEBUG|rg::args|crates/core/args.rs:626: error building Rust regex in hybrid mode:
regex parse error:
    (?<!\w)foo
    ^^^^
error: look-around, including look-ahead and look-behind, is not supported
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|rg::search|crates/core/search.rs:334: smaller: binary detection: BinaryDetection(Convert(0))
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:662: Some("smaller"): searching via memory map
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:761: slice reader: searching via multiline strategy
smaller: PCRE2: error matching: UTF-8 error: 1 byte missing at end

$ rg '(?<!\w)foo' -U -P smaller --trace
DEBUG|rg::config|crates/core/config.rs:40: /home/andrew/.ripgreprc: arguments loaded from config file: ["--max-columns-preview", "--colors=match:bg:0xff,0x7f,0x00", "--colors=match:fg:white", "--colors=line:none", "--colors=line:fg:magenta", "--colors=path:fg:green", "--type-add=got:*_test.go"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--max-columns-preview", "--colors=match:bg:0xff,0x7f,0x00", "--colors=match:fg:white", "--colors=line:none", "--colors=line:fg:magenta", "--colors=path:fg:green", "--type-add=got:*_test.go", "(?<!\\w)foo", "-U", "-P", "smaller", "--trace"]
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|rg::search|crates/core/search.rs:334: smaller: binary detection: BinaryDetection(Convert(0))
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:662: Some("smaller"): searching via memory map
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:755: slice reader: needs transcoding, using generic reader
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:719: generic reader: reading everything to heap for multiline
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:723: generic reader: searching via multiline strategy
1:foo
```

In particular, the former command winds up using a memory map, but the latter command seems to get into a place where it thinks transcoding is needed. That baffled me, so I started looking at the code for differences between the "auto" path and the `-P/--pcre2` path.

When it comes to PCRE2, it (or it did) requires valid UTF-8 when its Unicode mode is enabled, which ripgrep enables by default. PCRE2 has its own UTF-8 checking, although it is quite slow. So if possible, ripgrep will use a separate library for streaming UTF-8 decoding that is quite a bit faster.

Part of this case analysis for choosing an encoding depends on checking whether PCRE2 is being used or not. At time of writing, that check is implemented with this:

```
fn pcre2_unicode(&self) -> bool {
    self.is_present("pcre2") && self.unicode()
}
```

And there in lay the problem: the `pcre2` flag isn't set, because it was automatically enabled via `--engine auto`. This returned false and winded up causing our encoding choice to pick "none." Since it picked "none," it kept PCRE2's default UTF-8 checking enabled. (Otherwise, providing invalid UTF-8 would be undefined behavior.) And _that_ seems to be provoking the bug.

When `--pcre2` is set instead, then the UTF-8 encoding is selected and PCRE2's UTF-8 checking is disabled, and that it turn masks this bug.

But still, why is PCRE2 reporting a UTF-8 error if the file is valid UTF-8? After digging into it, it looks like ripgrep is for some reason passing a truncated fragment to PCRE2 that is indeed invalid UTF-8 and that provokes the error.

I haven't tracked down why ripgrep is passing a fragment like this yet though.


---

_Comment by @BurntSushi on 2021-09-02 15:48_

Sigh. It looks like this is a [regression introduce in ripgrep 13](https://github.com/BurntSushi/ripgrep/commit/efd9cfb2fc1f0233de9eda4c03416d32ef2c3ce8#diff-43597a3c260647a67283feb2ed8d6e5b365b2c2483dcada97fc6e32cb46d3402), but the regression is coupled to another bug fix, so it's not as simple as rolling it back. In particular, this is the relevant part of the change: https://github.com/BurntSushi/ripgrep/blob/9b01a8f9ae53ebcd05c27ec21843758c2c1e823f/crates/printer/src/lib.rs#L77-L86

Probably one way to fix this here is to change the look-ahead context slicing to be UTF-8 aware so that it doesn't split a codepoint and produce an invalid UTF-8 fragment.

We should also fix the encoding selection to be consistent across both `--engine auto` and `--pcre2`. (That seems to also fix this this specific instance of the bug, although it's not quite clear why.)

---

_Comment by @ashidaharo on 2021-09-02 17:15_

To be honest, I'm not very good at programming, especially since I've never had to deal with bytecode directly, and I'm not familiar with the inner workings of ripgrep.
I'm also not familiar with the inner workings of ripgrep. Speaking as an outsider, I think it's a very difficult task to split a given bytecode into grapheme clusters of appropriate size.
If you're just splitting by code points, you can do it in a primitive way: if the first two bits of the last byte of the split region are 0x, then it's a proper endpoint; otherwise, you can read the next byte first, and proceed with the endpoint until the first two bits of the next byte are no longer 10...
In the case of regular expressions, in current Unicode there are cases where multiple code points are considered to be "one character", as in emoji, right?

---

_Comment by @BurntSushi on 2021-09-02 18:32_

This isn't about characters or graphemes or anything like that. The pre-existing kludge in ripgrep just needs to get a little less kludgy by ensuring it doesn't split a UTF-8 encoding of a codepoint when it slices the text in this particular case.

---

_Comment by @ashidaharo on 2021-09-03 03:10_

I'm not familiar with the inner workings of this software, or the specific Unicode byte representation, so I've failed to generate a string that would cause it, but I'm concerned that the same problem could occur with UTF-16 surrogate pairs.
Not only in UTF-8, but also in UTF-16, one code point can be 2 bytes or 4 bytes. So limiting the look-ahead range to 128 bytes could, in theory, accidentally cause surrogate pairs to be broken up?

---

_Comment by @ashidaharo on 2021-09-03 05:56_

I'm really sorry for repeatedly writing, deleting, and editing my comments, but my mind was a mess.
The following is only what I have learned from what is happening now.

# 1. By specifying the encoding, the error will not occur even if --engine auto is enabled.
```
ashida@DESKTOP-MTMA9GA:/mnt/d/test$ cat baz.txt
foo
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAπ
ashida@DESKTOP-MTMA9GA:/mnt/d/test$ rg '(?<!\w)foo' -U --encoding 'utf-8' --trace --engine auto bar.txt
DEBUG|rg::args|crates/core/args.rs:626: error building Rust regex in hybrid mode:
regex parse error:
    (?<!\w)foo
    ^^^^
error: look-around, including look-ahead and look-behind, is not supported
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|rg::search|crates/core/search.rs:334: bar.txt: binary detection: BinaryDetection(Convert(0))
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:662: Some("bar.txt"): searching via memory map
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:755: slice reader: needs transcoding, using generic reader
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:719: generic reader: reading everything to heap for multiline
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:723: generic reader: searching via multiline strategy
1:foo
```
We can find out what it says about transcoding here, as we did when we set -P.
It seems that the strict Unicode check of PCRE2 is disabled when Unicode is specified as the encoding, even if --engine auto is left as enable.
# 2. There is another problem with the behavior when handing over a UTF-16 file without specifying the encoding.
The following two files were prepared for experiments to cause similar errors with surrogate pairs in UTF-16.
```ashida@DESKTOP-MTMA9GA:/mnt/d/test$ file -i bar1.txt
bar1.txt: text/plain; charset=utf-16be
ashida@DESKTOP-MTMA9GA:/mnt/d/test$ xxd bar1.txt
00000000: feff 0066 006f 006f 000a d842 dfb7 d842  ...f.o.o...B...B
00000010: dfb7 d842 dfb7 d842 dfb7 d842 dfb7 d842  ...B...B...B...B
00000020: dfb7 d842 dfb7 d842 dfb7 d842 dfb7 d842  ...B...B...B...B
00000030: dfb7 d842 dfb7 d842 dfb7 d842 dfb7 d842  ...B...B...B...B
00000040: dfb7 d842 dfb7 d842 dfb7 d842 dfb7 d842  ...B...B...B...B
00000050: dfb7 d842 dfb7 d842 dfb7 d842 dfb7 d842  ...B...B...B...B
00000060: dfb7 d842 dfb7 d842 dfb7 d842 dfb7 d842  ...B...B...B...B
00000070: dfb7 d842 dfb7 d842 dfb7 d842 dfb7 d842  ...B...B...B...B
00000080: dfb7 d842 dfb7 d842 dfb7 d842 dfb7 d842  ...B...B...B...B
00000090: dfb7 d842 dfb7 d842 dfb7 d842 dfb7 d842  ...B...B...B...B
000000a0: dfb7 d842 dfb7 d842 dfb7 d842 dfb7 d842  ...B...B...B...B
000000b0: dfb7 d842 dfb7 d842 dfb7 d842 dfb7 000a  ...B...B...B....

ashida@DESKTOP-MTMA9GA:/mnt/d/test$ file -i bar2.txt
bar2.txt: text/plain; charset=utf-16be
ashida@DESKTOP-MTMA9GA:/mnt/d/test$ xxd bar2.txt
00000000: feff 0066 006f 006f 000a 0041 d842 dfb7  ...f.o.o...A.B..
00000010: d842 dfb7 d842 dfb7 d842 dfb7 d842 dfb7  .B...B...B...B..
00000020: d842 dfb7 d842 dfb7 d842 dfb7 d842 dfb7  .B...B...B...B..
00000030: d842 dfb7 d842 dfb7 d842 dfb7 d842 dfb7  .B...B...B...B..
00000040: d842 dfb7 d842 dfb7 d842 dfb7 d842 dfb7  .B...B...B...B..
00000050: d842 dfb7 d842 dfb7 d842 dfb7 d842 dfb7  .B...B...B...B..
00000060: d842 dfb7 d842 dfb7 d842 dfb7 d842 dfb7  .B...B...B...B..
00000070: d842 dfb7 d842 dfb7 d842 dfb7 d842 dfb7  .B...B...B...B..
00000080: d842 dfb7 d842 dfb7 d842 dfb7 d842 dfb7  .B...B...B...B..
00000090: d842 dfb7 d842 dfb7 d842 dfb7 d842 dfb7  .B...B...B...B..
000000a0: d842 dfb7 d842 dfb7 d842 dfb7 d842 dfb7  .B...B...B...B..
000000b0: d842 dfb7 d842 dfb7 d842 dfb7 d842 dfb7  .B...B...B...B..
000000c0: 000a                                     ..
```
The two files have a sequence of 4-byte characters on the second line that is clearly longer than the 128-byte boundary at which errors occur, and differ only in the presence or absence of an A at the beginning of the second line.
So if there was going to be an error in the UTF-16 surrogate pair, it was expected that one of these two files would cause it.
This is the result.
```
ashida@DESKTOP-MTMA9GA:/mnt/d/test$ rg '(?<!\w)foo' -U --trace --engine auto bar1.txt
DEBUG|rg::args|crates/core/args.rs:626: error building Rust regex in hybrid mode:
regex parse error:
    (?<!\w)foo
    ^^^^
error: look-around, including look-ahead and look-behind, is not supported
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|rg::search|crates/core/search.rs:334: bar1.txt: binary detection: BinaryDetection(Convert(0))
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:662: Some("bar1.txt"): searching via memory map
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:755: slice reader: needs transcoding, using generic reader
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:719: generic reader: reading everything to heap for multiline
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:723: generic reader: searching via multiline strategy
1:foo

ashida@DESKTOP-MTMA9GA:/mnt/d/test$ rg '(?<!\w)foo' -U --trace --engine auto bar2.txt
DEBUG|rg::args|crates/core/args.rs:626: error building Rust regex in hybrid mode:
regex parse error:
    (?<!\w)foo
    ^^^^
error: look-around, including look-ahead and look-behind, is not supported
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|rg::search|crates/core/search.rs:334: bar2.txt: binary detection: BinaryDetection(Convert(0))
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:662: Some("bar2.txt"): searching via memory map
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:755: slice reader: needs transcoding, using generic reader
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:719: generic reader: reading everything to heap for multiline
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:723: generic reader: searching via multiline strategy
bar2.txt: PCRE2: error matching: UTF-8 error: 1 byte missing at end
```
The result for bar2.txt is clearly unusual. The file appears to have been processed as UTF-8 because of the "UTF-8 error", even though it is clearly a UTF-16BE with BOM. On the contrary, in this case, although the "slice reader: needs transcoding" is processed, the error seems to be different from the previous case.
However, in the case of bar1.txt, the result shows that it correctly matches "foo", which means that in this case, UTF-16BE was correctly identified and processed.
It is even more unusual to have inconsistent behavior when you are passing almost the same file with the same options.
I have confirmed that this same problem occurs with UTF-16LE as well.

---

_Comment by @ashidaharo on 2021-09-04 01:47_

I was able to read a bit of the source code when I had a free moment.
Apparently, the second problem I mentioned above solved itself when I found out that the files were all converted to UTF-8 internally. It seems that the error was not a separate problem, as it turns out that the error was probably caused by the same thing that happened in this Issue, so I apologize for the weirdness.
I also found out that this does not cause the surrogate pair problem I mentioned earlier. Again, my apologies for this.


---

_Comment by @ashidaharo on 2021-09-04 01:57_

I was also able to pinpoint the **correct** cause of the problem, which was that it behaved differently depending on whether the encoding was specified or not, and that not only that, but **forcing PCRE2 was hiding this bug.**
There is a small mistake here.
https://github.com/BurntSushi/ripgrep/blob/9b01a8f9ae53ebcd05c27ec21843758c2c1e823f/crates/core/args.rs#L494-L508
The case of "EncodingMode::Auto" is missing here; the case of "EncodingMode::Auto" does not mean that Unicode files are never given.
Therefore (apart from the problem of an invalid UTF-8 fragment, which was the original problem of this issue), the following fixes are needed.
```
 fn has_explicit_encoding(&self) -> bool { 
     match self { 
         EncodingMode::Some(_) => true, 
         EncodingMode::Auto => true,
         EncodingMode::None => false, 
     } 
 } 
```
This notation, which takes advantage of Rust's match exhaustiveness, can prevent further bugs from future extensions (although I don't see any such thing being added here).

---

_Comment by @ashidaharo on 2021-09-04 02:23_

I'm misreading the intent of this too!
If anything, stop the slow PCRE2 unicode check if you can, when transcoding is done internally. This is because it is guaranteed to convert to correct UTF-8.

---

_Comment by @ashidaharo on 2021-09-04 02:38_

> We should also fix the encoding selection to be consistent across both `--engine auto` and `--pcre2`.

The reason for this is that when "--pcre2" is set and no encoding is specified, "EncodingMode::Some("utf-8")" is automatically set.
Therefore, if you set "--encoding 'auto'" and "--pcre2", the same error will occur as with "--engine auto".
This is the process.
https://github.com/BurntSushi/ripgrep/blob/9b01a8f9ae53ebcd05c27ec21843758c2c1e823f/crates/core/args.rs#L1058-L1082

---

_Comment by @ashidaharo on 2021-09-04 02:54_

https://github.com/BurntSushi/ripgrep/blob/9b01a8f9ae53ebcd05c27ec21843758c2c1e823f/crates/searcher/src/searcher/mod.rs#L795-L799
This is where we determine if transcoding is required. It should be noted here that if the encoding is set to "auto" and UTF-8 without BOM is given, transcoding is not performed.
Yes, such handling is unavoidable, but anyway, if the encoding setting is "auto", the possibility of giving invalid UTF-8 can not be denied, so the current prce2 utf check should not be disabled.

---

_Comment by @ashidaharo on 2021-09-04 03:30_

Oh...and let's get back to the original problem of this issue.
> Probably one way to fix this here is to change the look-ahead context slicing to be UTF-8 aware so that it doesn't split a codepoint and produce an invalid UTF-8 fragment.

Currently impossible. This is because this program itself does not currently detect whether the given text is UTF-8 without BOM or not.

---

_Comment by @ashidaharo on 2021-09-04 04:43_

I thought of two solutions to this.
1. Programs should always, at all times, decode and treat the file as a string or transcode it to UTF-8, instead of treating it as a byte array.
    This solution can guarantee three things.
    1. the Unicode check can be done (probably) at least as fast as with pcre2.
    2. since we know that the processed string or UTF-8 bytes will come, it is easier to specify the appropriate lookahead range.
    3. it solves the problem of handling different encodings for each engine found, and provides consistent behavior at all times.
    
    Needless to say, however, this would not be in the spirit of the tool, as it would be very costly to handle non-Unicode strings and valid UTF-8 files without BOM.
2. A simple Unicode check should be performed even if there is no BOM, if it is equivalent to --encoding 'auto' being set.
    Needless to say, this has performance implications, but it is (probably, maybe, hopefully) lighter than the previous one when dealing with non-Unicode strings, and it is the only way to get the information needed to determine the correct lookahead range for UTF-8 without changing the way the file is handled as a string of bytes.
    However, I think this solution is even worse than the one before. The performance of this solution is worse than the previous one because it adds an extra step to check before transcoding when dealing with Unicode strings. Also, this solution does not verify the validity of originally valid UTF-8 files, so like the previous solution, it either adds the hassle of transcoding such files to the current process, or leaves the checking to PCRE2 like the current process.
    This is very bad for people who work only with non-Unicode files, especially ASCII, but I think the first way is better because it is better at working with Unicode files.

**Please note the following!** To my horror, the Unicode specification allows for no BOM, even for UTF-16 and UTF-32.! So goddamn it, the problem of not being able to detect whether a file is Unicode or not when setting "--encode 'auto'" is not limited to UTF-8.

---

_Comment by @BurntSushi on 2021-09-04 04:56_

I appreciate all the comments, but I already gave the solution in my last comment. It will fix this problem.

The more general problem of handling look ahead correctly is more difficult and has absolutely nothing to do with encoding. It's about abstraction boundaries between interfaces not taking look around into account.

---

_Comment by @ashidaharo on 2021-09-04 05:07_

That's good to know. I'm sorry for the massive amount of pointless garbage I've been dropping...

---

_Comment by @BurntSushi on 2023-11-24 20:16_

This should be fixed in a different way since PCRE2 can now search invalid UTF-8.

---

_Closed by @BurntSushi on 2023-11-24 20:16_

---

_Label `bug` added by @BurntSushi on 2023-11-24 20:16_

---
