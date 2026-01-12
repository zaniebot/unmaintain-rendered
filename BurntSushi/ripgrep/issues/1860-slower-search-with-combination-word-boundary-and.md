```yaml
number: 1860
title: Slower search with combination word boundary and multiple regex on certain files
type: issue
state: closed
author: ethack
labels: []
assignees: []
created_at: 2021-05-01T04:14:22Z
updated_at: 2021-05-02T03:21:40Z
url: https://github.com/BurntSushi/ripgrep/issues/1860
synced_at: 2026-01-12T16:13:24Z
```

# Slower search with combination word boundary and multiple regex on certain files

---

_@ethack_

#### What version of ripgrep are you using?

```bash
rg --version
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

```
apt-get -y install ripgrep
```

#### What operating system are you using ripgrep on?

Ripgrep is inside a docker container.
Host: Pop!_OS 20.04 (Ubuntu 20.04 variant)
Container: Ubuntu 21.04 Hirsute
Kernel:
```
uname -mr
5.11.0-7614-generic x86_64
```

#### Describe your bug.

This may be a duplicate or related to #1760. I am getting much slower execution time when using word boundaries `\b` in a regex. The search performance is:
- Fast with a `\b` search pattern.
- Slow with a `\b` search pattern plus an additional pattern.
- Fast with a `\b` search pattern plus an additional pattern when `--no-unicode` is used.
- Fast with a `\b` search pattern plus an additional pattern on a differently formatted file (TSV vs JSON).

#### What are the steps to reproduce the behavior?

Here are the two files which can be used to reproduce the issue.
[conn.json.log](https://github.com/BurntSushi/ripgrep/files/6408803/conn.json.log)
[conn.tsv.log](https://github.com/BurntSushi/ripgrep/files/6408805/conn.tsv.log)

These are [Zeek](https://zeek.org/) conn log files that contain the same information but in different formats. `conn.tsv.log` is tab-separated and was converted to JSON to generate `conn.json.log`. The files I uploaded are the `head -1000` of two larger files. I can provide the larger logs if they would be useful.

```
# original larger logs
du -h conn.tsv.log
54M conn.tsv.log
wc -l conn.tsv.log
455545 conn.tsv.log

du -h conn.json.log
150M	conn.json.log
wc -l conn.json.log
455536 conn.json.log
```

The `time` command differences are very small with the files I uploaded, but I'll also provide [hyperfine](https://github.com/sharkdp/hyperfine) output for both the full logs and the ones I uploaded.

#### What is the actual behavior?

The following timings are on the larger logs.

When I use a regex for an IP address, the search time is very small (<1s).
```
$ time rg -e '\b10\.55\.182\.100\b' conn.json.log >/dev/null
rg -e '\b10\.55\.182\.100\b' conn.json.log > /dev/null  0.73s user 0.16s system 381% cpu 0.231 total
```

When I add an additional regex, the search time is long (>30sec).
```
$ time rg -e '^#' -e '\b10\.55\.182\.100\b' conn.json.log >/dev/null
rg -e '^#' -e '\b10\.55\.182\.100\b' conn.json.log > /dev/null  151.99s user 1.41s system 401% cpu 38.231 total
```

If I add `--no-unicode`, the search time drops back down (<1s).
```
$ time rg --no-unicode -e '^#' -e '\b10\.55\.182\.100\b' conn.json.log >/dev/null
rg --no-unicode -e '^#' -e '\b10\.55\.182\.100\b' conn.json.log > /dev/null  0.28s user 0.01s system 98% cpu 0.300 total
```

This next part puzzles me even more. Recall that the TSV file was used to generate the JSON file. I know these aren't the same and are quite different file sizes. But it's not like a bunch of non-ASCII characters got added to one file and not the other. If I run the command from the JSON file that caused the long search time on the TSV, the search time is short (<1s).
```
$ time rg -e '^#' -e '\b10\.55\.182\.100\b' conn.tsv.log >/dev/null
rg -e '^#' -e '\b10\.55\.182\.100\b' conn.tsv.log > /dev/null  0.12s user 0.01s system 99% cpu 0.131 total
```

Here are hyperfine results that compare the last three commands, along with using `--no-unicode` on the TSV file for good measure.

```
$ hyperfine -w 1 -i \
  -n tsv-unicode "rg -e '^#' -e '\b10\.55\.182\.100\b' conn.tsv.log" \
  -n tsv-no-unicode "rg --no-unicode -e '^#' -e '\b10\.55\.182\.100\b' conn.tsv.log" \
  -n json-unicode "rg -e '^#' -e '\b10\.55\.182\.100\b' conn.json.log" \
  -n json-no-unicode "rg --no-unicode -e '^#' -e '\b10\.55\.182\.100\b' conn.json.log"

Benchmark #1: tsv-unicode
  Time (mean ± σ):     150.1 ms ±   7.5 ms    [User: 531.0 ms, System: 54.8 ms]
  Range (min … max):   133.6 ms … 166.4 ms    22 runs
 
Benchmark #2: tsv-no-unicode
  Time (mean ± σ):     156.6 ms ±   8.9 ms    [User: 559.2 ms, System: 57.2 ms]
  Range (min … max):   144.6 ms … 188.8 ms    19 runs
 
Benchmark #3: json-unicode
  Time (mean ± σ):     46.889 s ±  1.209 s    [User: 187.340 s, System: 1.998 s]
  Range (min … max):   45.547 s … 49.531 s    10 runs
 
Benchmark #4: json-no-unicode
  Time (mean ± σ):     508.9 ms ±  20.1 ms    [User: 1.863 s, System: 0.186 s]
  Range (min … max):   462.8 ms … 531.0 ms    10 runs
 
Summary
  'tsv-unicode' ran
    1.04 ± 0.08 times faster than 'tsv-no-unicode'
    3.39 ± 0.22 times faster than 'json-no-unicode'
  312.32 ± 17.55 times faster than 'json-unicode'
```

And here are hyperfine results with nearly the same commands on the smaller files provided, just to show that the same differences can be seen.

```
$ hyperfine -w 1 \
  -n rg-tsv "rg -e '^#' -e '\b10\.55\.182\.100\b' conn.tsv.log | wc -l" \
  -n rg-json "rg -e '^#' -e '\b10\.55\.182\.100\b' conn.json.log | wc -l" \
  -n rg-json-no-unicode "rg --no-unicode -e '^#' -e '\b10\.55\.182\.100\b' conn.json.log | wc -l"

Benchmark #1: rg-tsv
  Time (mean ± σ):       5.0 ms ±   2.0 ms    [User: 4.0 ms, System: 1.7 ms]
  Range (min … max):     2.8 ms …  11.6 ms    866 runs
 
  Warning: Command took less than 5 ms to complete. Results might be inaccurate.
 
Benchmark #2: rg-json
  Time (mean ± σ):      59.8 ms ±   5.6 ms    [User: 58.6 ms, System: 1.6 ms]
  Range (min … max):    51.5 ms …  70.9 ms    50 runs
 
Benchmark #3: rg-json-no-unicode
  Time (mean ± σ):       6.0 ms ±   2.1 ms    [User: 4.8 ms, System: 2.0 ms]
  Range (min … max):     3.1 ms …  11.7 ms    775 runs
 
  Warning: Command took less than 5 ms to complete. Results might be inaccurate.
 
Summary
  'rg-tsv' ran
    1.20 ± 0.63 times faster than 'rg-json-no-unicode'
   11.89 ± 4.76 times faster than 'rg-json'
```

Here are several debug outputs.

Fast (one search pattern):
```
$ rg --debug  -e '\b10\.55\.182\.100\b' conn.json.log >/dev/null         
DEBUG|grep_regex::literal|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/grep-regex-0.1.8/src/literal.rs:180: required literal found: "10.55.182.100"
DEBUG|grep_regex::matcher|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/grep-regex-0.1.8/src/matcher.rs:50: extracted fast line regex: (?-u:10\x2e55\x2e182\x2e100)
DEBUG|globset|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/globset-0.4.5/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

Slow (two search patterns):
```
$ rg --debug -e '^#' -e '\b10\.55\.182\.100\b' conn.json.log >/dev/null
DEBUG|globset|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/globset-0.4.5/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

Fast (two search patterns, --no-unicode):
```
$ rg --debug --no-unicode -e '^#' -e '\b10\.55\.182\.100\b' conn.json.log >/dev/null
DEBUG|globset|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/globset-0.4.5/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

Fast (two search patterns, TSV):
```
$ rg --debug -e '^#' -e '\b10\.55\.182\.100\b' conn.tsv.log >/dev/null             
DEBUG|globset|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/globset-0.4.5/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

Fast (two search patterns, no word boundaries):
```
$ rg --debug -e '^#' -e '10\.55\.182\.100' conn.json.log >/dev/null
DEBUG|globset|/usr/share/cargo/registry/ripgrep-12.1.1/debian/cargo_registry/globset-0.4.5/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

#### What is the expected behavior?

Ideally, I'd like the slow search to take roughly the same time as the other fast searches.

I understand from #1760 that word boundaries with Unicode is a slow process. But if that's the case here, I wonder why the same search on a different file can be so much faster?

---

_Closed by @BurntSushi on 2021-05-01 11:45_

---

_Comment by @BurntSushi on 2021-05-01 11:53_

This is the definition of a perfect bug report. Thank you so much. And thank you for reporting it. This was indeed quite a subtle performance bug in the regex engine. The actual fix is here: https://github.com/rust-lang/regex/pull/768

The higher level view is that there is a space (and also time) saving optimization in the faster lazy DFA engine that groups together bytes into equivalence classes that can't otherwise discriminate between a match and a non-match. That was actually correct here, but the grouping didn't account for bytes that cause the DFA to quit early. In particular, your JSON file has many `{` (that's `0x7B`) bytes. It's very close to the upper end of the ASCII range, which increases its likelihood of getting lumped together with bytes outside of the ASCII range. And indeed, that's exactly what happened here. The regex engine was continually running the lazy DFA, seeing a `{`, stopping incorrectly and then falling back to a slower NFA engine.

The fix for this was to simply mark the ASCII range as distinct from the non-ASCII range when constructing the equivalence class boundaries (if a Unicode word boundary is seen).

Another interesting way to look at this, is if you try adding the `\{\}$` regex to your search. That will incidentally result in a set of equivalence classes where brackets aren't seen as non-ASCII, and thus, the search runs quickly.

Thanks again for the great bug report!

---

_Comment by @ethack on 2021-05-02 03:21_

Thank you for the explanation and quick turn-around for a fix! It sounds like there are some really neat things happening in the rust regex library. Ripgrep has become an indispensable part of my toolkit so you have my gratitude and admiration for all your work on the project. 

---
