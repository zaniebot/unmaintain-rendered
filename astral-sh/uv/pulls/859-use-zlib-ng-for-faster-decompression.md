```yaml
number: 859
title: Use zlib-ng for faster decompression
type: pull_request
state: merged
author: bojanserafimov
labels:
  - performance
assignees: []
merged: true
base: main
head: bojan/use-zlib-ng
created_at: 2024-01-09T20:24:56Z
updated_at: 2024-01-11T16:00:44Z
url: https://github.com/astral-sh/uv/pull/859
synced_at: 2026-01-10T15:39:02Z
```

# Use zlib-ng for faster decompression

---

_Pull request opened by @bojanserafimov on 2024-01-09 20:24_

ty @BurntSushi for the suggestion

# Bench results

Summary: 20-25% speedup in cases like flyte.txt and pdm-2913.txt, where compression is important. Other cases are either saturating my bandwidth or using source packages. No major regressions.

### scripts/bench/requirements.txt
main:
Resolved 45 packages in 709ms
Downloaded 45 packages (total size: 14586813 bytes) in 438ms (rate: 31.758675 MB/sec)
Num source packages: 0
Installed 45 packages in 57ms

ng:
Downloaded 45 packages in 428ms

### black.txt (compiled from scripts/requirements/black.in)
main:
Resolved 8 packages in 100ms
Downloaded 8 packages (total size: 1960918 bytes) in 129ms (rate: 14.4835825 MB/sec)
Num source packages: 0
Installed 8 packages in 22ms

ng:
Downloaded 8 packages in 81ms 

### boto3.txt (compiled from scripts/requirements/boto3.in)
Resolved 31 packages in 919ms
Downloaded 31 packages (total size: 39488261 bytes) in 1.52s (rate: 24.74746 MB/sec)
Num source packages: 0
Installed 31 packages in 70ms

ng:
Downloaded 31 packages in 1.26s

### dtlssocket.txt
Resolved 2 packages in 182ms
Downloaded 2 packages (total size: 2066977 bytes) in 4.98s (rate: 0.395072 MB/sec)
Num source packages: 1
Installed 2 packages in 48ms

ng:
Downloaded 2 packages in 4.94s

### flyte.txt
Resolved 107 packages in 1.51s
Downloaded 107 packages (total size: 1417831287 bytes) in 1m 02s (rate: 21.669163 MB/sec)
Num source packages: 0
Installed 107 packages in 338ms

ng:
Downloaded 107 packages in 45.64s

### pdm-2913.txt
Resolved 13 packages in 261ms
Downloaded 13 packages (total size: 31291075 bytes) in 1.32s (rate: 22.568693 MB/sec)
Num source packages: 1
Installed 13 packages in 29ms

ng:
Downloaded 13 packages in 1.08s

### scispacy.txt
Resolved 84 packages in 1.32s
Downloaded 84 packages (total size: 223013396 bytes) in 6.09s (rate: 34.916824 MB/sec)
Num source packages: 0
Installed 84 packages in 111ms

ng:
Downloaded 84 packages in 6.15s

### trio.txt
Resolved 38 packages in 370ms
Downloaded 38 packages (total size: 25506995 bytes) in 739ms (rate: 32.887703 MB/sec)
Num source packages: 0
Installed 38 packages in 61ms

ng:
Downloaded 38 packages in 703ms


---

_@charliermarsh approved on 2024-01-09 20:27_

That's an excellent speedup, thanks for testing across a bunch of scenarios.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-09 20:27_

---

_Label `performance` added by @charliermarsh on 2024-01-09 20:27_

---

_Merged by @bojanserafimov on 2024-01-09 21:13_

---

_Closed by @bojanserafimov on 2024-01-09 21:13_

---

_Branch deleted on 2024-01-09 21:13_

---

_@BurntSushi reviewed on 2024-01-10 13:34_

Nice work!

---

_Comment by @bojanserafimov on 2024-01-11 16:00_

I retested this because previously I was using a debug build (see https://github.com/astral-sh/puffin/pull/856#issuecomment-1884016311). The improvement is small but real

```
python -m scripts.bench \
    --puffin-path ./target/release/puffin-main \
    --puffin-path ./target/release/puffin-no-zlib \
    --benchmark install-cold \
    --min-runs 100 \
    scripts/requirements/compiled/black.txt

Summary
  ./target/release/puffin-main (install-cold) ran
    1.05 ± 0.29 times faster than ./target/release/puffin-no-zlib (install-cold)
```


---
