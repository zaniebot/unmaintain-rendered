```yaml
number: 804
title: Enable self-benchmarking for Puffin branches in bench.py
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/puffin-main
created_at: 2024-01-05T22:11:33Z
updated_at: 2024-01-06T15:44:23Z
url: https://github.com/astral-sh/uv/pull/804
synced_at: 2026-01-12T16:04:12Z
```

# Enable self-benchmarking for Puffin branches in bench.py

---

_@charliermarsh_

## Summary

This PR enables the use of the `bench.py` script to benchmark Puffin itself. This is something I often do by via a process like:

- Checkout the `main` branch (or any other baseline branch)
- Run: `cargo build --release`
- Run: `mv ./target/release/puffin ./target/release/baseline`
- Checkout a development branch
- Run: `cargo build --release`
- (New) Run: `python bench.py --tool puffin --path ./target/release/puffin --tool puffin --path ./target/release/baseline requirements.in`


---

_@zanieb approved on 2024-01-05 22:15_

Cool!

---

_Comment by @charliermarsh on 2024-01-05 22:17_

Is this better or worse than an API wherein you can specify the Puffin binaries yourself? Like: `python scripts/bench.py requirements.in --binary puffin --binary baseline`

---

_Comment by @zanieb on 2024-01-05 22:28_

Specifying the binary paths manually seems a bit nicer, but I don't quite see how it would work with your example? Don't you need to specify both the enum member and the path?

---

_Comment by @charliermarsh on 2024-01-05 23:30_

Yeah, you do need to specify both, but Puffin is included by default as a member so it can be omitted.

---

_Comment by @charliermarsh on 2024-01-05 23:30_

I'll make that change. Then I have another idea for making these composable so that we can get a single Hyperfine report, which will be much nicer.

---

_Comment by @zanieb on 2024-01-05 23:31_

To clarify, in `--binary puffin --binary baseline` where am I specifying the path to the binary and where am I saying which tool the binary is for?

---

_Comment by @charliermarsh on 2024-01-05 23:32_

They're both Puffin binaries, just compiled from different branches (by you, out-of-band). They _have_ to be Puffin binaries.

---

_Comment by @zanieb on 2024-01-06 00:33_

Hm it's not really clear which one is the baseline in that case. I guess I'd expect to be able to set the path to _any_ of the tools. e.g. `--<enum> <path>` or `--<enum>-binary <path>`.

---

_Comment by @charliermarsh on 2024-01-06 02:42_

Good idea, I will try to do something more general.

---

_Marked ready for review by @charliermarsh on 2024-01-06 03:10_

---

_Merged by @charliermarsh on 2024-01-06 03:23_

---

_Closed by @charliermarsh on 2024-01-06 03:23_

---

_Branch deleted on 2024-01-06 03:23_

---

_@zanieb reviewed on 2024-01-06 05:53_

---

_Review comment by @zanieb on `scripts/bench/__main__.py`:724 on 2024-01-06 05:53_

This seems like an awkward API. If you want to provide a path for one tool you must provide a path for all tools? If you provide a path without specifying tools it'll apply to the first default tool?

---

_@charliermarsh reviewed on 2024-01-06 11:50_

---

_Review comment by @charliermarsh on `scripts/bench/__main__.py`:724 on 2024-01-06 11:50_

They're `None`-padded parallel arrays. I originally allowed `--tool puffin:./path/to/binary` but that felt even worse since you then don't get argparse validation or help messages since the argument is custom. What do you prefer? Your original suggestion seemed to require that we have command-line arguments for every tool (like `--pip`) which strikes me as structurally wrong.

---

_@charliermarsh reviewed on 2024-01-06 13:59_

---

_Review comment by @charliermarsh on `scripts/bench/__main__.py`:724 on 2024-01-06 13:59_

I think I will just change back to `script --enum value1=/path/to/file1 --enum value2=/path/to/file2` or `script --enum value1:path/to/file1 --enum value2:path/to/file2` (where the path is optional and omitted) given that this seems confusing. The downside is that we can't use `choices` (since we now parse arbitrary string values), and you also don't get autocomplete on tab to the file when writing out the command on the CLI since it's part of a composed string argument.

We can't allow `script --enum value1 /path/to/file1 --enum value2 /path/to/file2` since we need to accept the requirements file as a positional argument so this would introduce ambiguity (is `/path/to/file1` the path to the binary, or the requirements file?).

---

_@zanieb reviewed on 2024-01-06 15:44_

---

_Review comment by @zanieb on `scripts/bench/__main__.py`:724 on 2024-01-06 15:44_

While having a command line argument for every tool feels structurally weird, it's definitely the most usable as it should have all of the hints you'd expect. It's pretty trivial to iterate over the choices to generate the options. I feel like since it's an internal tool with a reasonably small upper bound on the number of choices I'd go with this. 

The `:` split parsing seems hard to use. I wouldn't go back to that :/

Anyway probably doesn't matter to much! Easy enough to change latter. Do what you want :)

---
