```yaml
number: 1201
title: "--version doesn't reflect the downloaded version"
type: issue
state: closed
author: axbender
labels: []
assignees: []
created_at: 2019-02-21T09:07:43Z
updated_at: 2019-02-21T16:19:50Z
url: https://github.com/BurntSushi/ripgrep/issues/1201
synced_at: 2026-01-12T16:13:23Z
```

# --version doesn't reflect the downloaded version

---

_@axbender_

#### What version of ripgrep are you using?
0.1.2 supposedly, but:
ripgrep 0.10.0 (rev fc3cf41247)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?
Archive extraction.

#### What operating system are you using ripgrep on?
Windows 10

#### Describe your question, feature request, or bug.
The result of rg --version doesn't match the downloaded version.

---

_Comment by @BurntSushi on 2019-02-21 10:41_

Huh? That version output looks correct. Please provide more details. I don't understand what's wrong here. Please be very specific about what the expected output is versus the actual output. Please also provide more specific details on what exactly you downloaded. Show the URL if you have to. 

I can't make sense of this issue in its current form.

---

_Comment by @axbender on 2019-02-21 15:55_

Hm, probably the report was based on a misunderstanding on my part...

I downloaded "grep-searcher-0.1.2" (as it was the first in the queue), unpacked it (it contains just "rg.exe"), and was wondering why the downloaded version "0.1.2" was not reflected in the output of --version. 

Obviously (and to my surprise) the ripgrep and grep-searcher releases bear the same executable name but differ in some respect (could you shed some light on this?).

---

_Comment by @BurntSushi on 2019-02-21 16:19_

Because grep-searcher isn't actually a release. The CI configuration for Windows a new release for every tag and I don't always remember to clean it up.

It should be clear which release is the primary one because it actually has release notes.

---

_Closed by @BurntSushi on 2019-02-21 16:19_

---
