```yaml
number: 2115
title: "Feature Request: add option for \"in place\" find and replace"
type: issue
state: closed
author: ElectricRCAircraftGuy
labels:
  - duplicate
assignees: []
created_at: 2022-01-02T06:32:40Z
updated_at: 2022-01-04T07:29:45Z
url: https://github.com/BurntSushi/ripgrep/issues/2115
synced_at: 2026-01-12T16:13:24Z
```

# Feature Request: add option for "in place" find and replace

---

_@ElectricRCAircraftGuy_

#### Describe your feature request

Please add a `-i` / `--in-place` option (like in `sed`) to edit files in place. I'd really like to use ripgrep for find and replace across code bases, since it's so fast, but currently it requires some pretty difficult work-arounds to accomplish the desired behavior. 

Ex: for a single file I do this to replace all instances of `blue` with `red` in file `ip.txt`:
```bash
file_contents_and_stats="$(rg --stats --passthru 'blue' -r 'red' ip.txt)"
NUM_STATS_LINES=9
file_contents="$(head -n -$NUM_STATS_LINES "$file_contents_and_stats")"
stats="$(tail -n $NUM_STATS_LINES "$file_contents_and_stats")"
printf "%s" "$file_contents" > ip.txt
printf "%s" "$stats"
```

...but when trying to find across many files, this becomes much much harder, and I must first obtain a file list with `rg -l` then operate on that.

---

_Comment by @BurntSushi on 2022-01-02 06:56_

Dupe of #74.

---

_Closed by @BurntSushi on 2022-01-02 06:56_

---

_Label `duplicate` added by @BurntSushi on 2022-01-02 06:56_

---

_Renamed from "Feature Request: add -i option for "in place" find and replace" to "Feature Request: add option for "in place" find and replace" by @ElectricRCAircraftGuy on 2022-01-02 15:32_

---

_Comment by @ElectricRCAircraftGuy on 2022-01-04 07:28_

For anyone looking for this feature, see here; I just wrote a wrapper and added it: https://github.com/BurntSushi/ripgrep/issues/74#issuecomment-1004581477

---
