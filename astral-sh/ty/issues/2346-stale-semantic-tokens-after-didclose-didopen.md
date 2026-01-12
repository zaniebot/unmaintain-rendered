```yaml
number: 2346
title: Stale Semantic Tokens After didClose/didOpen
type: issue
state: closed
author: joaotavora
labels:
  - bug
  - great-writeup
  - server
assignees: []
created_at: 2026-01-05T15:32:11Z
updated_at: 2026-01-06T10:52:54Z
url: https://github.com/astral-sh/ty/issues/2346
synced_at: 2026-01-12T15:54:26Z
```

# Stale Semantic Tokens After didClose/didOpen

---

_@joaotavora_

### Summary

When an LSP documention is closed and immediately reopened by the editor with completely different content (simulating an external file modification detected via file watchers), `ty`'s response to the subsequent `textDocument/semanticTokens/full` response returns stale data from the previously opened version.

1. Open a Python file with this content (Version A):
```python
def calculate_sum(a):
    # Version A: Basic math
    return a

result = calculate_sum(5)
```

2. Request semantic tokens - server correctly responds with tokens for this content
3. Close the file via `textDocument/didClose`
4. Immediately reopen the same file via `textDocument/didOpen` with completely different content (Version B):
```python
# Version B: Basic greeting
def say_hello():
    print("Hello, World!")

say_hello()
```

5. Request semantic tokens again

The expectation is that the server returns semantic tokens matching the new content (Version B with `say_hello` function).  

Instead, it  returns the exact same semantic tokens data from the previous version (Version A with `calculate_sum` function).

The current workaround is to kill the ty session and connect again.

In the attached transcript.

1. Initial didOpen with Version A:
   - Content: `"def calculate_sum(a):\n # Version A: Basic math\n return a\n\nresult = calculate_sum(5)\n"`

2. First semanticTokens/full[3] response:
   - Result: `{"data":[0,4,13,7,1,0,14,1,2,1,2,11,1,2,0,2,0,6,5,1,0,9,13,7,0,0,14,1,11,0]}`

3. didClose followed by didOpen with Version B:
   - Content: `"# Version B: Basic greeting\ndef say_hello():\n print(\"Hello, World!\")\n\nsay_hello()\n"`

4. Second semanticTokens/full[8] response:
   - Result: `{"data":[0,4,13,7,1,0,14,1,2,1,2,11,1,2,0,2,0,6,5,1,0,9,13,7,0,0,14,1,11,0]}`
   - **← IDENTICAL to response from step 2!**

The semantic tokens data is byte-for-byte identical despite the file content being completely different. This causes incorrect syntax highlighting in the editor.

[transcript.txt](https://github.com/user-attachments/files/24436164/transcript.txt)

### Version

ty 0.0.9

---

_Label `great-writeup` added by @MichaReiser on 2026-01-05 15:34_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-05 15:35_

---

_Label `bug` added by @MichaReiser on 2026-01-05 15:35_

---

_Label `server` added by @MichaReiser on 2026-01-05 15:35_

---

_Comment by @MichaReiser on 2026-01-05 15:38_

Thanks for the great write-up and the transcript. Can you tell us what editor/client you're using?

I suspect the issue is that the second `didOpen` request re-uses the `version: 0`. This is likely to break our caching because we use the version to check for changes since we last read the file (in `source_text` by depending on `revision`). 

We might need to include a timestamp to disambiguate file versions, given that they aren't unique. Unless there's something else going on, why we don't propagate the change correctly to Salsa



---

_Comment by @Gankra on 2026-01-05 15:41_

From the logs this appears to be https://github.com/joaotavora/eglot ?

---

_Comment by @MichaReiser on 2026-01-05 15:57_

From @Gankra and my DM's. 

What I would expect to happen is:

* file is open: revision is 0
* file gets closed: revision is set to the timestamp on disk
* file gets opened: revision is 0

So there should be at least 2 writes to `File::revision` that, in turn, invalidate Salsa's caching. We should add an E2E test for this exact scenario and log why `File::sync` isn't updating the revision (or if it is, why the request remain stale). 



---

_Closed by @MichaReiser on 2026-01-06 10:52_

---
