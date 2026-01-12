```yaml
number: 1833
title: "received unknown options for workspace ^file:///D:/Master_Jobs/AI_Study /code/product-classify' : 'ty'. Refer to the logs for more details."
type: issue
state: closed
author: hjqaixuexi
labels:
  - question
  - server
assignees: []
created_at: 2025-12-10T05:44:51Z
updated_at: 2025-12-10T07:10:06Z
url: https://github.com/astral-sh/ty/issues/1833
synced_at: 2026-01-12T15:54:25Z
```

# received unknown options for workspace ^file:///D:/Master_Jobs/AI_Study /code/product-classify' : 'ty'. Refer to the logs for more details.

---

_@hjqaixuexi_

### Question

In Zed,Ireceived unknown options for workspace ^file:///D:/Master_Jobs/AI_Study
/code/product-classify' : 'ty'. Refer to the logs for more details.

### Version

_No response_

---

_Label `question` added by @hjqaixuexi on 2025-12-10 05:44_

---

_Label `server` added by @AlexWaygood on 2025-12-10 05:56_

---

_Comment by @dhruvmanila on 2025-12-10 06:09_

Can you provide the logs? They should be available in `dev: open language server logs` from the command palette. Make sure to select the "ty" language server if you have other language servers configured as well.

Maybe we should include the unknown options that the server found: https://github.com/astral-sh/ruff/blob/aaadf16b1b39ec7c742a5183ddc2cc4e2af34bab/crates/ty_server/src/server.rs#L97-L122

Oh, I also think we should remove the "HACK" before the beta. I can open up PRs for that.

---

_Label `needs-info` added by @dhruvmanila on 2025-12-10 06:09_

---

_Comment by @dhruvmanila on 2025-12-10 06:11_

Oh wait, the unknown options should be visible in the popup message as it is:

> file:///D:/Master_Jobs/AI_Study/code/product-classify' : 'ty'.

Ok, I think I know where the issue might be. Are you providing settings like the following?

```jsonc
{
  "lsp": {
    "ty": {
      "settings": {
        "ty": {  // <-- remove this
          "diagnosticMode": "workspace"
        }
      }
    }
  }
}
```

You need to update it to remove the "ty" section:

```json
{
  "lsp": {
    "ty": {
      "settings": {
        "diagnosticMode": "workspace"
      }
    }
  }
}
```

---

_Label `needs-info` removed by @dhruvmanila on 2025-12-10 06:16_

---

_Closed by @hjqaixuexi on 2025-12-10 06:35_

---

_Comment by @hjqaixuexi on 2025-12-10 06:36_

},
  "restore_on_startup": "none",
  "languages": {
    "Python": {
      "show_edit_predictions": true,
      "language_servers": ["!pylsp", "ty", "ruff", "!pyright", "!basedpyright"]
    }
this is my setting of lsp @dhruvmanila 

---

_Comment by @dhruvmanila on 2025-12-10 06:38_

Do you have any other settings apart from this specific to the ty server?

---

_Comment by @hjqaixuexi on 2025-12-10 06:44_

> Do you have any other settings apart from this specific to the ty server?

Only these，So I find it very strange, haha.

---

_Comment by @hjqaixuexi on 2025-12-10 06:46_

Everything works fine with the functionality of the ty, except that there is this prompt every time you turn it on

---

_Comment by @dhruvmanila on 2025-12-10 06:50_

What's the Zed version that you're using? Are you using the builtin ty extension? Can you try it on the latest Zed version?

---

_Comment by @hjqaixuexi on 2025-12-10 06:57_

Wow, I'm so sorry, I suddenly realized there was this part. I've corrected it as you suggested, and everything is fine now. Thank you so much, my eyes are really terrible!

```
 "lsp": {
    "ty": {
      "settings": {
        "ty": {
          "disableLanguageServices": false,
          "diagnosticMode": "openFilesOnly",
          "inlayHints": {
            "variableTypes": true,
            "callArgumentNames": true
          },
          "experimental": {
            "rename": false
          }
        }
      }
    }
```

---

_Comment by @MichaReiser on 2025-12-10 07:10_

We're glad you found the problematic settings. Enjoy ty

---
