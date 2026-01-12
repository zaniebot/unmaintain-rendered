```yaml
number: 2868
title: "better block device & huge file handling: seek & parallel"
type: issue
state: closed
author: robbat2
labels:
  - wontfix
assignees: []
created_at: 2024-08-10T21:57:58Z
updated_at: 2024-08-11T11:17:38Z
url: https://github.com/BurntSushi/ripgrep/issues/2868
synced_at: 2026-01-12T16:13:25Z
```

# better block device & huge file handling: seek & parallel

---

_@robbat2_

#### Describe your feature request

I'm using ripgrep to scan large block devices for various string patterns, and I want the byte offset of the pattern in my results. I don't need a line number, because it's meaningless.

I want to be able to start/resume the search

This also lends itself very well to parallel searching, if the block device is fast enough (e.g. NVME has enough sequential IO to saturate CPUs).

I can do a hacky implementation today with `dd` first, and manual chunking, but that means the byte offsets must be adjusted relative to the start of the chunk.

At a minimum, there should be a "--skip $OFFSET" and "--max-read $BYTES" options, that when given a block device, seeks to that position in the device before starting to read. This option will implicitly disable line counting, and enable byte offset output.
It would read at most `max-read` bytes before exiting. Any results output should use the absolute position in the block device in their byte offset output.

Lastly, in the output of such a mode, output an occasional no-matches entry, that describes the completed region of the block device that was scanned WITHOUT finding any results.

Manual parallel scan can be implemented on top of that functionality initially, as well as potential automated resume.

```
# quick resume
rg -f input-strings --skip-bytes $offset /dev/sdX --line-buffered --byte-offset --json --no-line-number >results.json

# quick parallel search
seq 0 20 | xargs -I^ -P4 sh -c 'rg -f input-strings --skip-bytes ^TiB --max-bytes 1TiB /dev/sdX --line-buffered --byte-offset --json --no-line-number >results.^TiB.json'
```

---

_Comment by @BurntSushi on 2024-08-10 22:11_

I think this would be better served in a new distinct tool. There are a lot of UX corners you aren't really addressing with this request. For example, what happens when a directory is given or more than one path or block device?

---

_Comment by @robbat2 on 2024-08-10 22:28_

I agree that those cases have some ambiguous corners; but nothing insurmountable.

- multiple block devices: short of integrated option vs arg parsing, the most meaningful thing to do in the fact of ambiguous input is to quit with a clear message.
```
# this COULD be implemented depending on the argparser, but it's a trivial conversion for the user to run it as separate commands.
rg --skip-bytes 0TiB --max-bytes 1TB /dev/sda  --skip-bytes 1TiB --max-bytes 1TB /dev/sdb
```

- `ripgrep`, when given a directory, already does not scan block devices in that directory - it silently skips them (knowing it skipped devices so may be valuable to some users and should probably be raised as a separate feature request).

```
# recursive in directory
$ sudo rg -m1 -e . /dev/block/  --json
{"data":{"elapsed_total":{"human":"0.002828s","nanos":2828076,"secs":0},"stats":{"bytes_printed":0,"bytes_searched":0,"elapsed":{"human":"0.000000s","nanos":0,"secs":0},"matched_lines":0,"matches":0,"searches":0,"searches_with_match":0}},"type":"summary"}

# explicitly passing some devices
$ sudo rg -m1 -e . /dev/block/253:{0,1}  --json
{"type":"begin","data":{"path":{"text":"/dev/block/253:0"}}}
{"type":"match","data":{"path":{"text":"/dev/block/253:0"},"lines":{"text":"2\n"},"line_number":1027,"absolute_offset":1026,"submatches":[{"match":{"text":"2"},"start":0,"end":1}]}}
{"type":"end","data":{"path":{"text":"/dev/block/253:0"},"binary_offset":0,"stats":{"elapsed":{"secs":0,"nanos":76594,"human":"0.000077s"},"searches":1,"searches_with_match":1,"bytes_searched":3,"bytes_printed":243,"matched_lines":1,"matches":1}}}
{"type":"begin","data":{"path":{"text":"/dev/block/253:1"}}}
{"type":"match","data":{"path":{"text":"/dev/block/253:1"},"lines":{"text":"d\n"},"line_number":1027,"absolute_offset":1026,"submatches":[{"match":{"text":"d"},"start":0,"end":1}]}}
{"type":"end","data":{"path":{"text":"/dev/block/253:1"},"binary_offset":0,"stats":{"elapsed":{"secs":0,"nanos":278543,"human":"0.000279s"},"searches":1,"searches_with_match":1,"bytes_searched":3,"bytes_printed":243,"matched_lines":1,"matches":1}}}
{"data":{"elapsed_total":{"human":"0.002995s","nanos":2995030,"secs":0},"stats":{"bytes_printed":486,"bytes_searched":6,"elapsed":{"human":"0.000355s","nanos":355137,"secs":0},"matched_lines":2,"matches":2,"searches":2,"searches_with_match":2}},"type":"summary"}
```

---

_Comment by @robbat2 on 2024-08-10 22:30_

I'd go as far as to say: when using `--skip-bytes`, `ripgrep` must be given exactly one input file.

---

_Comment by @BurntSushi on 2024-08-11 11:17_

I think there are too many odd UX corners to this use case for it to be supported by ripgrep. And it seems very likely that this falls into the bucket of "a feature that begets features." I think this could be much more effectively built as a separate tool. Remember that riprep's core infrastructure is exposed as a series of Rust libraries (although their docs aren't great).

---

_Closed by @BurntSushi on 2024-08-11 11:17_

---

_Label `wontfix` added by @BurntSushi on 2024-08-11 11:17_

---
