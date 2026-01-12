```yaml
number: 601
title: "diagnostics don't update until the language server is restarted on windows"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - server
  - windows
assignees: []
created_at: 2025-06-07T08:29:47Z
updated_at: 2025-06-29T09:29:13Z
url: https://github.com/astral-sh/ty/issues/601
synced_at: 2026-01-12T15:54:23Z
```

# diagnostics don't update until the language server is restarted on windows

---

_@DetachHead_

### Summary

when the language server initially starts, the correct diagnostics are returned, however when subsequent changes are made to the document that would change the diagnostics, the language server still returns the outdated diagnostics.

### to reproduce

1. create a python file with  a type error in it:
    ```py
    asdf 
    ```
2. (re)start the ty language server
3. see the `textDocument/diagnostic` result:
    ```
    [Trace - 6:14:49 PM] Sending request 'textDocument/diagnostic - (18)'.
    Params: {
        "identifier": "ty",
        "textDocument": {
            "uri": "file:///c%3A/Users/user/Documents/asdfads/test.py"
        }
    }
    
    
    [Trace - 6:14:49 PM] Received response 'textDocument/diagnostic - (18)' in 1ms.
    Result: {
        "items": [
            {
                "code": "unresolved-reference",
                "codeDescription": {
                    "href": "https://ty.dev/rules#unresolved-reference"
                },
                "message": "Name `asdf` used when not defined",
                "range": {
                    "end": {
                        "character": 4,
                        "line": 0
                    },
                    "start": {
                        "character": 0,
                        "line": 0
                    }
                },
                "relatedInformation": [],
                "severity": 1,
                "source": "ty"
            }
        ],
        "kind": "full"
    }
    ```
5. delete/comment out the line with the type error:   
   ```py
   # asdf
   ```
6. check the new `textDocument/diagnostics` response:
    ```
    [Trace - 6:15:15 PM] Sending request 'textDocument/diagnostic - (29)'.
    Params: {
        "identifier": "ty",
        "textDocument": {
            "uri": "file:///c%3A/Users/user/Documents/asdfads/test.py"
        }
    }
    
    
    [Trace - 6:15:15 PM] Received response 'textDocument/diagnostic - (29)' in 1ms.
    Result: {
        "items": [
            {
                "code": "unresolved-reference",
                "codeDescription": {
                    "href": "https://ty.dev/rules#unresolved-reference"
                },
                "message": "Name `asdf` used when not defined",
                "range": {
                    "end": {
                        "character": 4,
                        "line": 0
                    },
                    "start": {
                        "character": 0,
                        "line": 0
                    }
                },
                "relatedInformation": [],
                "severity": 1,
                "source": "ty"
            }
        ],
        "kind": "full"
    }
    ```

![Image](https://github.com/user-attachments/assets/3f288962-a17d-4e15-ab5a-3964495f16ab)

### environment

ty vscode extension v2025.15.11531247
OS: windows 10

### Version

0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Label `server` added by @AlexWaygood on 2025-06-07 08:47_

---

_Comment by @MichaReiser on 2025-06-07 08:48_

Interesting. There should have been at least one didChange notification from the client to the server to notify it about the updated file content 

---

_Comment by @DetachHead on 2025-06-07 08:55_

it looks like `textDocument/didChange` is being sent

<details><summary>here's the full trace from when i commented out the line</summary>
<p>

```
[Trace - 6:52:48 PM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/user/Documents/asdfads/test.py",
        "version": 40
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 0,
                    "character": 0
                },
                "end": {
                    "line": 0,
                    "character": 0
                }
            },
            "rangeLength": 0,
            "text": "# "
        }
    ]
}


[Trace - 6:52:48 PM] Sending request 'textDocument/diagnostic - (7)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/user/Documents/asdfads/test.py"
    }
}


[Trace - 6:52:48 PM] Received response 'textDocument/diagnostic - (7)' in 1ms.
Result: {
    "items": [
        {
            "code": "unresolved-reference",
            "codeDescription": {
                "href": "https://ty.dev/rules#unresolved-reference"
            },
            "message": "Name `asdf` used when not defined",
            "range": {
                "end": {
                    "character": 4,
                    "line": 0
                },
                "start": {
                    "character": 0,
                    "line": 0
                }
            },
            "relatedInformation": [],
            "severity": 1,
            "source": "ty"
        }
    ],
    "kind": "full"
}


[Trace - 6:52:48 PM] Sending request 'textDocument/inlayHint - (8)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/user/Documents/asdfads/test.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 0,
            "character": 6
        }
    }
}


[Trace - 6:52:48 PM] Received response 'textDocument/inlayHint - (8)' in 3ms.
Result: []


[Trace - 6:52:48 PM] Sending request 'textDocument/diagnostic - (9)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/user/Documents/asdfads/typings/test.pyi"
    }
}


[Trace - 6:52:48 PM] Received response 'textDocument/diagnostic - (9)' in 4ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 6:52:48 PM] Sending request 'textDocument/inlayHint - (10)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/user/Documents/asdfads/test.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 0,
            "character": 6
        }
    }
}


[Trace - 6:52:48 PM] Received response 'textDocument/inlayHint - (10)' in 3ms.
Result: []


[Trace - 6:52:50 PM] Sending request 'textDocument/diagnostic - (11)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/user/Documents/asdfads/typings/test.pyi"
    }
}


[Trace - 6:52:50 PM] Received response 'textDocument/diagnostic - (11)' in 2ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 6:52:50 PM] Sending request 'textDocument/diagnostic - (12)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/user/Documents/asdfads/test.py"
    }
}


[Trace - 6:52:50 PM] Received response 'textDocument/diagnostic - (12)' in 1ms.
Result: {
    "items": [
        {
            "code": "unresolved-reference",
            "codeDescription": {
                "href": "https://ty.dev/rules#unresolved-reference"
            },
            "message": "Name `asdf` used when not defined",
            "range": {
                "end": {
                    "character": 4,
                    "line": 0
                },
                "start": {
                    "character": 0,
                    "line": 0
                }
            },
            "relatedInformation": [],
            "severity": 1,
            "source": "ty"
        }
    ],
    "kind": "full"
}
```

</p>
</details> 



---

_Comment by @MichaReiser on 2025-06-07 13:04_

Hmm, I need to test this on Windows. It seems to be working fine on mac os

---

_Comment by @MichaReiser on 2025-06-07 13:48_

I can reproduce this on Windows. The problem is that the URL -> path and path -> URL aren't loseless. 

URL sent from server: `/c%3A/Users/Micha/astral/test/create.py`
Converted to path: `c:\Users\Micha\astral\test\create.py`
Path converted to URL: `/C:/Users/Micha/astral/test/create.py`

Note the URL encoding for `:` in the original URL and the lower case `c` that now is an uppercase `C`

---

_Label `windows` added by @AlexWaygood on 2025-06-07 13:51_

---

_Label `bug` added by @AlexWaygood on 2025-06-07 13:51_

---

_Comment by @MichaReiser on 2025-06-07 14:01_

Fixing this might be a bit more work but probably fairly important. 

My feeling is that we should rewrite our `Index`. The way it is built makes sense for Ruff, but not for ty. I think we should use `File` as lookup key instead of `Url`s. We may need something extra for notebook cells but that's where my knowledge is very limited. Doing so would hopefully also reduce some of the somewhat awkward url -> path and path -> url conversions (Ideally we only ever go from URL to path but never back)

I'm curious to hear @dhruvmanila's opinion.

---

_Renamed from "diagnostics don't update until the language server is restarted" to "diagnostics don't update until the language server is restarted on windows" by @DetachHead on 2025-06-07 14:08_

---

_Comment by @MichaReiser on 2025-06-07 14:17_

Actually, using `File` might not be possible because the server's `System` implementation needs to lookup document's by path. However, `AnyPath` (I don't recall the exact name of the enum) might be a suitable replacement for `Url`. It's not clear if we want to store the `File` on the document for easier lookup

I think this should work pretty well. We can store the url on `TextDocument `and `NotebookDocument` so that we can back when needed. We could also consider storing `File` on `TextDocument` and `NotebookDocument`. 

Most code should then directly use `DocumentKey` (which can have a `path` method) instead of having to resolve the key and the path.

---

_Comment by @dhruvmanila on 2025-06-09 08:33_

> I think this should work pretty well. We can store the url on `TextDocument `and `NotebookDocument` so that we can back when needed. We could also consider storing `File` on `TextDocument` and `NotebookDocument`.

The issue with this is that the way `NotebookDocument` is structured, we require the `Url` to get the `NotebookCell`. I thought of improving the notebook structure a while ago but couldn't find the time, I might do something when we work on completing the notebook support in the ty server.

> Most code should then directly use `DocumentKey` (which can have a `path` method) instead of having to resolve the key and the path.

To avoid going back from path to url, it would be useful to have a data structure that holds both. The `DocumentKey` seems like something that could be used for that as a `Url` can be used to create it which will then store the `Url` as well. This doesn't work right now because the `LSPSystem` requires the path to lookup the document as you've noticed.

---

_Closed by @MichaReiser on 2025-06-09 14:39_

---

_Comment by @DetachHead on 2025-06-29 09:29_

this issue seems to be mostly fixed, but it seems to still occur in some cases. i've been trying for hours to narrow this down to a minimal repo, but i can't figure it out unfortunately, so the steps to reproduce involve cloning my repo:

1. clone https://github.com/DetachHead/pytest-robotframework/commit/360a3b3ad171d80f019fe4f26e17d313d4c63164
2. run `./pw uv sync`
3. open `pytest_robotframework/__init__.py` in vscode
4. remove [one of these `# ty:ignore` comments](https://github.com/DetachHead/pytest-robotframework/blob/360a3b3ad171d80f019fe4f26e17d313d4c63164/pytest_robotframework/__init__.py#L272-L273) and notice that the error does not come back
5. restart the language server and see that the error comes back

---
