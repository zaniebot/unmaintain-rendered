---
number: 2188
title: "`-v` / `--verbose` isn't user-friendly"
type: issue
state: closed
author: not-my-profile
labels:
  - cli
assignees: []
created_at: 2023-01-26T06:22:58Z
updated_at: 2023-02-03T18:31:38Z
url: https://github.com/astral-sh/ruff/issues/2188
synced_at: 2026-01-07T13:12:14-06:00
---

# `-v` / `--verbose` isn't user-friendly

---

_Issue opened by @not-my-profile on 2023-01-26 06:22_

```
$ echo 'import foo' > bar.py
$ ruff bar.py -v
[2023-01-26][07:21:36][globset][DEBUG] built glob set; 19 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
[2023-01-26][07:21:36][globset][DEBUG] built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
[2023-01-26][07:21:36][ruff::commands][DEBUG] Identified files to lint in: 488.396µs
[2023-01-26][07:21:36][ruff::commands][DEBUG] Checked files in: 569.952µs
bar.py:1:8: F401 `foo` imported but unused
Found 1 error.
1 potentially fixable with the --fix option.
```

As a user I really don't care about DEBUG messages. I think it would make much more sense to instead show what `--show-source` currently shows:

```
bar.py:1:8: F401 `foo` imported but unused
  |
1 | import foo
  |        ^^^ F401
  |
  = help: Remove unused import: `foo`

Found 1 error.
1 potentially fixable with the --fix option.
```

---

_Referenced in [astral-sh/ruff#2149](../../astral-sh/ruff/issues/2149.md) on 2023-01-26 06:24_

---

_Renamed from "`-v` / `--verbose` is user-hostile" to "`-v` / `--verbose` isn't user-friendly" by @not-my-profile on 2023-01-26 09:50_

---

_Comment by @charliermarsh on 2023-01-26 14:11_

<img width="859" alt="Screen Shot 2023-01-26 at 9 10 30 AM" src="https://user-images.githubusercontent.com/1309177/214857113-1e61de84-b4fc-44ef-9398-01196f0be538.png">

😂 😂 😂

---

_Comment by @charliermarsh on 2023-01-26 14:21_

I think `--show-source` would be a surprising behavior for `-v`. But I've gone back and forth on the _right_ behavior as different tools treat `-v` differently.

For example, Mypy shows you all sorts of info related to the cache:

<img width="868" alt="Screen Shot 2023-01-26 at 9 11 35 AM" src="https://user-images.githubusercontent.com/1309177/214857373-4c33045f-02d8-4a78-b280-df664e565fe2.png">

So that's very similar to what we do now.

With `-v`, Black lists every file and whether it was changed or not (as opposed to only listing changed files), but doesn't show any "debug" info (dissimilar to what we do now).

With `-v`, isort tells you how every import was categorized (something we do now).

If I could enumerate the things we include with `-v`:

- `globset` stuff (unintended, would be happy to remove this from any and all logging)
- Sub-step benchmarks (not critical)
- Whether a file was cached (useful)
- Whether a file excluded and why (useful)
- How imports were categorized and why (useful)

I _could_ see us moving all of that to `--debug`. Though I'm unsure what `-v` would then include, and what the use-case for it would be. Would we expect `-v` to be a setting that some users have enabled permanently?

---

_Label `cli` added by @charliermarsh on 2023-01-26 14:48_

---

_Comment by @charliermarsh on 2023-01-26 14:56_

To answer my own question, `-v` could include:

- The number of fixes applied to each file (#2149)
- ...anything else?

---

_Comment by @charliermarsh on 2023-01-26 14:56_

While we're here, it'd be nice if `-v` also showed you whether rules were ignored due to `--per-file-ignores`.

---

_Comment by @not-my-profile on 2023-01-26 17:50_

I want to note that a primary reason for opening this issue was that I find `--show-source` to be very useful (it's much more userfriendly than `--diff`) but the flag is currently very verbose. And `-s` is already occupied as a short flag for `--silent`.

I also don't consider `show-source` to be a great name for the feature since `--diff` also shows the source ... just in a different format. Semantically I think it makes sense to consider this output to be more verbose than the default output.

> it'd be nice if -v also showed you whether rules were ignored due to --per-file-ignores

I guess that could be useful but probably rather in combination with `--show-files`, right? I don't think users always want to see this when desiring the current `--show-source` output.

---

_Comment by @JonathanPlasse on 2023-01-26 19:06_

There could be multiple verbosity levels, with the highest one reserved for debug information.

---

_Comment by @VictorGob on 2023-02-02 06:52_

> To answer my own question, `-v` could include:
> 
> * The number of fixes applied to each file ([CLI: Show `--fix`ed and fixable issues too? #2149](https://github.com/charliermarsh/ruff/issues/2149))
> * ...anything else?

* Number of files processed.


Change it to `debug!("Checked {:?} files in: {:?}", file_count, duration);`
https://github.com/charliermarsh/ruff/blob/7a83b65fbeed85a51975fc31d3c292ff192fb2ef/ruff_cli/src/commands.rs#L139

I would use "--show-files" to see _exactly_ which files have been processed. 

**EDIT:** 
Actually, it's just:
`debug!("Checked {:?} files in: {:?}", paths.len(), duration);`

Tested directly in the project folder +  `01.py` file created by me.
```
root@a2965381d438:/data# ./target/debug/ruff -v *.py --show-files --no-cache
[2023-02-02][09:54:56][globset][DEBUG] built glob set; 19 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
/data/01.py
/data/setup.py
[2023-02-02][09:54:56][ruff::commands][DEBUG] Identified files to lint in: 4.254566ms
[2023-02-02][09:54:56][ruff::commands][DEBUG] Checked 2 files in: 4.794871ms
01.py:1:8: F401 `os` imported but unused
Found 1 error.
1 potentially fixable with the --fix option.
root@a2965381d438:/data# 
```

---

_Referenced in [astral-sh/ruff#2506](../../astral-sh/ruff/pulls/2506.md) on 2023-02-03 00:38_

---

_Comment by @charliermarsh on 2023-02-03 18:31_

I think I'm okay with the current state of this, for now at least. I removed the `globset` logging, so we only show first-party stuff. We could consider making `--verbose` `--debug` instead.

---

_Closed by @charliermarsh on 2023-02-03 18:31_

---
