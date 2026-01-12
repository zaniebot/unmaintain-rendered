```yaml
number: 2246
title: "emit log message when file is skipped becaue it was detected as \"binary\" data"
type: issue
state: closed
author: asomers
labels:
  - enhancement
  - rollup
assignees: []
created_at: 2022-06-24T15:42:29Z
updated_at: 2023-11-25T20:03:59Z
url: https://github.com/BurntSushi/ripgrep/issues/2246
synced_at: 2026-01-12T16:13:24Z
```

# emit log message when file is skipped becaue it was detected as "binary" data

---

_@asomers_

#### What version of ripgrep are you using?
```
ripgrep 13.0.0
+SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?
Through the FreeBSD ports collection

#### What operating system are you using ripgrep on?

FreeBSD 14.0-CURRENT amd64.  The file system is ZFS.

#### Describe your bug.

I have a directory with 552,900 entries, one for every version of every crate ever published to crates.io.  If I run `rg` with no PATH arguments, it finishes in about a minute with no results.  However, if I run it with specific PATH arguments then it finds plentiful results.  The likeliest explanation I can think of is that when recursing through `.`, rg doesn't iterate through every child.

#### What are the steps to reproduce the behavior?

First, create a fresh file system with at least 100 GB of space.  Then download every published crate, using a command similar to the following.  Note that `fetch` is a FreeBSD-specific command, and may be replaced by curl or wget.
```
git clone https:​//github.com/rust-lang/crates.io-index index
grep -hr . index/*/ | jq '.name + "-" + .vers + ".tar.gz " + "https://crates.io/api/v1/crates/" + .name + "/" + .vers + "/download"' -r | xargs -P100 -n2 fetch -o
```
Run this command to show which files match.  You can CTRL-C it after you're satisfied.
```
ls | xargs -n 1000 rg -l -z '\bsigevent\b'`
```
Then run this command.  It will wrongly produce no output.
```
rg -l -z '\bsigevent\b'
```
#### What is the actual behavior?

```
> rg --debug -l -z  '\bsigevent\b'
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:180: required literal found: "sigevent"
DEBUG|grep_regex::matcher|crates/regex/src/matcher.rs:50: extracted fast line regex: (?-u:sigevent)
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.affected-packages.txt.swp: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.summary.txt.swp: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./index/.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./index/.github: Ignore(IgnoreMatch(Hidden))
```

#### What is the expected behavior?

It should have returned about 1353 files.


---

_Comment by @BurntSushi on 2022-06-24 15:48_

Seems very likely that smart filtering is being applied here. Is the `--debug` log you posted complete? Because it should have the answer.

If it is a smart filtering issue then `rg -uuu -l -z '\bsigevent\b'` should show some results because `-uuu` disables all smart filtering.

---

_Comment by @asomers on 2022-06-24 17:04_

Yep!  Using `--binary` fixed the problem.  Might I suggest that `-z` should imply `--binary` ?

---

_Comment by @BurntSushi on 2022-06-24 17:09_

I don't see any reason why `-z` would imply `--binary`. All `-z` does is decompress files. That has nothing to do with whether the binary filter is used or not.

---

_Comment by @BurntSushi on 2022-06-24 17:10_

I'll leave this open as a bug for now (not because of `-z` not implying `--binary`), but because ideally, there would be something in the `--debug` logs telling you that files were being skipped because they were detected as binary.

---

_Label `enhancement` added by @BurntSushi on 2022-06-24 17:10_

---

_Renamed from "Can't recurse over half a million children" to "emit log message when file is skipped becaue it was detected as "binary" data" by @BurntSushi on 2022-06-24 17:10_

---

_Comment by @asomers on 2022-06-24 17:13_

Well, using `-z` implies that the user wants to search through binary files, because all compressed files are binary.  Without `--binary`, ripgrep won't search any compressed files.

---

_Comment by @BurntSushi on 2022-06-24 17:14_

The _compressed_ form of the file is binary, sure, but ripgrep isn't searching the compressed data. It's searching the _uncompressed_ data. And that might not be binary.

---

_Comment by @BurntSushi on 2022-06-24 17:15_

It also looks like you might be trying to search `.tar.gz` files. ripgrep doesn't search archives like you might be expecting it to.

---

_Comment by @asomers on 2022-06-24 17:39_

But it does search .tar.gz files.  For example:
```
> rg --binary -l -z  '\bsigevent\b'
pgx-pg-sys-0.1.1.tar.gz
rust-mio-0.1.0.tar.gz
libc-0.2.80.tar.gz
...
```
Do you mean that it won't display the matching lines?  I wouldn't expect that.  That's why I'm using `-l`.

---

_Comment by @BurntSushi on 2022-06-24 17:58_

I didn't say ripgrep doesn't search .tar.gz files. Please read what I said more carefully. What I said was, ripgrep doesn't search archives like you might be expected it to. ripgrep searches `.tar` files like any other kind of file. What it doesn't do is extract archives into its constituent parts.

ripgrep sees the `.gz` extension and decompresses the `.tar.gz` file. That decompressed file is a tar archive. That tar archive, _in its decompressed state_, is itself a binary file. ripgrep will treat it like a binary file. This is correct and its **totally and completely orthogonal to whether it was originally compressed or not**.

The fact that a `.tar.gz` file decompresses to a binary file is a consequent of that specific file. But, say, `dict.txt.gz` decompresses to a plain text file with no binary data at all.

I don't know how to be any more clear than this. `-z/--search-zip` and `--binary` are totally and completely and 100% orthogonal.

---

_Comment by @asomers on 2022-06-24 18:08_

Yes, I wasn't expecting ripgrep to extract the tar files.  But that was OK, because I was using `-l`.  The part I didn't get was the difference between a .tar.gz and a .txt.gz file.  Now I understand why -z shouldn't imply --binary .

---

_Label `rollup` added by @BurntSushi on 2023-11-24 19:56_

---

_Closed by @BurntSushi on 2023-11-25 20:03_

---
