```yaml
number: 328
title: experiment with Windows memory map flags
type: issue
state: closed
author: BurntSushi
labels:
  - help wanted
  - question
  - icebox
assignees: []
created_at: 2017-01-16T23:14:08Z
updated_at: 2021-05-09T03:25:33Z
url: https://github.com/BurntSushi/ripgrep/issues/328
synced_at: 2026-01-12T16:13:21Z
```

# experiment with Windows memory map flags

---

_@BurntSushi_

```
[18:08] * WindowsBunnyDancingOnRainbows wonders if burntsushi hints sequential access in ripgrep
[18:08] <WindowsBunnyDancingOnRainbows> Ralith: Well there are functions to hint to the OS you'll be using memory soon so it'll prefetch it
[18:09] <burntsushi> WindowsBunnyDancingOnRainbows: i don't do anything special with mmaps. i just open them and search.
[18:09] <WindowsBunnyDancingOnRainbows> burntsushi: You search sequentially though?
[18:09] <burntsushi> yes
[18:10] <WindowsBunnyDancingOnRainbows> burntsushi: Then you *really* want to pass FILE_FLAG_SEQUENTIAL_SCAN when opening the file
[18:10] <WindowsBunnyDancingOnRainbows> Both with and without mmaps
[18:10] <burntsushi> WindowsBunnyDancingOnRainbows: is that supported by the memmap crate?
[18:10] <WindowsBunnyDancingOnRainbows> burntsushi: Does memmap support opening from an fs::File?
[18:10] <burntsushi> i believe so
[18:11] <WindowsBunnyDancingOnRainbows> burntsushi: Then you can use the OpenOptionsExt on Windows to open an fs::File with that flag, then use it or pass it to memmap
```

---

_Label `help wanted` added by @BurntSushi on 2017-01-16 23:14_

---

_Label `question` added by @BurntSushi on 2017-01-16 23:14_

---

_Comment by @retep998 on 2017-02-02 03:01_

Also, note that `FILE_FLAG_SEQUENTIAL_SCAN` should be set on all your files, not just your memory maps.

---

_Comment by @BurntSushi on 2017-02-02 11:56_

@CannedYerins I think the only thing in `benchsuite` that is Linux only is building the Linux kernel.

If `benchsuite` is too inconvenient to run on Windows, then I think we'll need to come up with our own benchmark for this. It should include short and long files, as well as a small number of files and a large number of files.

---

_Comment by @retep998 on 2017-02-04 22:03_

If you run the benchmark over and over that flag won't have much difference because the file is already cached in memory. It is when you're reading a large file from disk and it hasn't been cached in memory yet, that is when there is a noticeable improvement. Instead of getting a big beefy machine with 30GB of ram, try using something with much less RAM and benchmark it on files that are *bigger* than your RAM (that way it can't cache the whole file in memory).

---

_Comment by @BurntSushi on 2017-02-18 19:26_

@CannedYerins Thanks for doing this! It looks like there is virtually no difference to me. You ran the benchmarks with only a single iteration, so it's impossible to see the variance to make a final determination. The other problem is that 4GB of RAM is enough to fit all of the benchmark corpora in memory. :-(

The other issue here is that you're only passing the seq scan flag when opening a file for buffered reading, but ripgrep is probably using memory maps in all of the subtitle benchmarks, which I think means you aren't actually measuring what you want to measure. :-( See the mmap logic here: https://github.com/BurntSushi/ripgrep/blob/master/src/args.rs#L564


---

_Label `icebox` added by @BurntSushi on 2017-04-09 13:11_

---

_Comment by @BurntSushi on 2018-01-31 02:57_

I'm going to close this because I don't see much value in tracking it. Given my ignorance of Windows, I wouldn't be surprised if someone could make ripgrep go faster via some Windows-specific tweaks. But I don't have the bandwidth to do it.

---

_Closed by @BurntSushi on 2018-01-31 02:57_

---
