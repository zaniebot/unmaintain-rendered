```yaml
number: 3035
title: "add built-in ability to limit search to first N bytes of file/stream (like with `head -c`)"
type: issue
state: open
author: moschroe
labels:
  - enhancement
assignees: []
created_at: 2025-04-18T14:37:06Z
updated_at: 2025-08-05T14:49:13Z
url: https://github.com/BurntSushi/ripgrep/issues/3035
synced_at: 2026-01-12T16:13:25Z
```

# add built-in ability to limit search to first N bytes of file/stream (like with `head -c`)

---

_@moschroe_

#### Describe your feature request

I recently had the use case of searching an nginx cache (using slicing) containing many 100 GiB of data. Each file contains the response HTTP header for the slice. The slices can be over a MiB in size (could vary depending on nginx config), but the content is irrelevant, all I needed was within the first 1-2KiB of every file.

It was a pain to find certain cache slices because ripgrep has to stream the entire file (it contains a binary header, so binary mode must be used) in case there is no match, which holds for the vast majority of files.

I thought of having a built-in `head -c` that would prevent needless examination of file contents beyond a known header.

An anecdotal benchmark on my system shows a factor 2000 lower search time with a cold filesystem cache and factor 400 with a warm cache. This is a completely different dataset with fewer, larger files, though, the original system is unfortunately no longer accessible to me.

I implemented a draft for this feature here, would love a review and, hopefully, merge: https://github.com/moschroe/ripgrep/tree/feat_head-bytes (should I open a PR?)
It is most likely not as clean as it could be, so I'd be happy to improve it until acceptable.

---

_Label `enhancement` added by @BurntSushi on 2025-04-18 14:45_

---

_Comment by @BurntSushi on 2025-04-18 14:46_

I think I'd be open to this. It would be pretty annoying to stitch this together using `head` outside of ripgrep. I suppose you could use `--pre` for this though. Have you considered that?

---

_Comment by @moschroe on 2025-04-18 17:56_

To be honest, I scoured the man page multiple times but, for some reason, must have overlooked or disregarded `--pre`. 🤦

While there is certainly some overhead to spawning ~1 million processes (1TB/1MB=1e6 files), it would most likely always beat having to read every file entirely by a large margin.

My main reason to use `rg` here was that I knew it could not only filter but also traverse the filesystem efficiently. But with the pre process being spawned on already discovered files, it would likely be efficient enough.

I might be able to benchmark the difference, should I get access to such a system again.

---

_Comment by @SaintNick2023 on 2025-08-05 14:47_

I'm sure there will be plenty of use cases for limiting fetch/read and search only the first N bytes of the files, especially when combined with --max-count 1 it would give a **huge** performance boost for many large files where the searchstring is in a header block.

---
