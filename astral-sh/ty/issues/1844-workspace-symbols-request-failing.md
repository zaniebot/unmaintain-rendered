```yaml
number: 1844
title: workspace symbols request failing
type: issue
state: open
author: insane-dreamer
labels:
  - needs-info
  - server
  - fatal
assignees: []
created_at: 2025-12-10T18:30:48Z
updated_at: 2025-12-31T15:41:25Z
url: https://github.com/astral-sh/ty/issues/1844
synced_at: 2026-01-12T15:54:25Z
```

# workspace symbols request failing

---

_@insane-dreamer_

### Summary

When using the "search symbol" feature in `zed` no results are shown, and `ty` panics with: 
```
2025-12-10T10:15:56-08:00 INFO  [lsp] starting language server process. binary path: "/home/sean/.local/share/zed/languages/ty/ty-0.0.1-alpha.33/ty-x86_64-unknown-linux-gnu/ty", working directory: "/home/sean/dev/cpe", args: ["server"]
2025-12-10T10:16:01-08:00 ERROR [crates/project/src/lsp_store.rs:7620] workspace symbols request

Caused by:
    request handler panicked at crates/ruff_source_file/src/line_index.rs:198:44:
    byte index 40962 is out of bounds of `"""
    import os
    import logging
    from ...engine import *
    import numpy as np
    
    from .channel_space import internalize_coo`[...]
    run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

the issue isn't specific to the above file. If I remove that file and restart the server, it errors on the next file it hits:
```
2025-12-10T10:21:31-08:00 ERROR [crates/project/src/lsp_store.rs:7620] workspace symbols request

Caused by:
    request handler panicked at crates/ruff_source_file/src/line_index.rs:198:44:
    byte index 12080 is out of bounds of `import unittest
    import logging
    import numpy as np
    from neuropype.engine import *
    from neuropype.nodes import ROIActivations, ROIsToVertices
    from neuropype.utilities.testing import expect_exception
    
    
    logger = logging.getLogger(__name__)
    
    
    class ROIsTestCase`[...]
    run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

In both cases the trace shows a backtick on the last line  ``` that is not in the actual code. 



### Version

0.0.1-alpha.33, on Ubuntu 24.04

---

_Label `server` added by @AlexWaygood on 2025-12-10 18:42_

---

_Label `fatal` added by @AlexWaygood on 2025-12-10 18:43_

---

_Comment by @MichaReiser on 2025-12-11 09:07_

Thanks for reporting. Curious. This either suggests that our internal document state is outdated or the byte index is indeed out of bounds. I tried workspace symbols across a few projects in Zed and unfortunately wasn't able to reproduce the issue.

Would you be able to share the entire source text of the file for which the request failed (the file containing `class ROIsTestCase`[...]`)?

The last backtick is from the panick message. There's also a backtick before the `import unittest`

---

_Label `needs-info` added by @MichaReiser on 2025-12-11 09:12_

---

_Comment by @insane-dreamer on 2025-12-11 17:09_

Here is the second file. I ran it again and confirmed it produced the same error.
Also:

- `basedpyright` works ok on this same project (with this same  file) -- and search by symbol works if I disable `ty` and select `basedpyright` in my settings. 
 
- `ty` works ok on other projects I tested (though it only partially finds all the symbols, but I'll file that as a separate issue)

 [test_rois.zip](https://github.com/user-attachments/files/24107909/test_rois.zip)

---

_Comment by @insane-dreamer on 2025-12-11 19:24_

@MichaReiser forgot to tag you in above response 

---

_Comment by @MichaReiser on 2025-12-11 19:26_

Tagging isn't necessary. I'm subscribed to all issues :)

Hmm, I tried your file but ty just doesn't want to crash...

---

_Comment by @insane-dreamer on 2025-12-11 20:37_

I tried moving the file I sent you into its own project and it works fine there. But it reliably crashes when used in the main project (which I can't post here). Could there be a stale `ty` index that needs to be reset on the project? How would I test that? 

---

_Comment by @MichaReiser on 2025-12-12 10:10_

> But it reliably crashes when used in the main project (which I can't post here)

Interesting. I wonder if ty ends up confusing two files. Do you use any symlinking in your project?

> Could there be a stale ty index that needs to be reset on the project? How would I test that?

ty doesn't use any persistent caching. Just restarting the server (or VS Code) should be enough to get a "clean" start



---

_Comment by @insane-dreamer on 2025-12-12 17:25_

@MichaReiser I made a clean pull of the repo with the offending project, and same issue. 

I'm deleting the files that `ty` is panicking on, and seeing a couple of patterns which may be clues.  I haven't found them all yet, so it's definitely not just one or two files that might somehow be corrupt. It's crashing on specific things in those files, though not when the file is in a project by its own. 

After deleting 7 files in the codebase I finally got it to where `ty` was no longer panicking and symbol search is working (not sure if it's returning all results but that's a separate issue). 

I noticed two patterns:  

-  a few of the files have an "unresolve-attribute" error flagged by `ty`:

<img width="1043" height="129" alt="Image" src="https://github.com/user-attachments/assets/ca003c07-a8b1-435b-9d53-039e3a8fedf4" />

- a couple other files have docstrings with non-ascii characters: 

<img width="782" height="106" alt="Image" src="https://github.com/user-attachments/assets/0431678c-8265-411f-8da0-2f125d0f506f" />

In these cases, I deleted the docstring, and `ty` no longer panicked on the file. So pretty sure that is one issue. 

On the other hand, one of the files it panicked on had neither of the above conditions -- couldn't see anything unusual with it at all. 

I can't post the files here, but hopefully the above helps to narrow it down. 

---

_Comment by @MichaReiser on 2025-12-12 17:52_

Thanks for going so deep on this investigation. Non ASCII characters are certainly a strong suspect. Out of interest, can you also reproduce the crashes when using VS Code?

---

_Comment by @insane-dreamer on 2025-12-12 18:07_

I don't use VSCode or have it installed, so couldn't say. 

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:41_

---
