```yaml
number: 1856
title: symbol search returns only partial list of matches
type: issue
state: closed
author: insane-dreamer
labels:
  - bug
  - server
assignees: []
created_at: 2025-12-11T17:44:31Z
updated_at: 2025-12-12T12:29:30Z
url: https://github.com/astral-sh/ty/issues/1856
synced_at: 2026-01-12T15:54:25Z
```

# symbol search returns only partial list of matches

---

_@insane-dreamer_

### Summary

When performing a symbol search in `zed` with `ty` enabled, I'm only getting a partial list of matches. I thought it was only returning the matches from open files, but I tried closing all files in the project, or opening other files, and the list of returned matches remained the same. Switching to `basedpyright` returns all matches. A couple of the matches that didn't show up with `ty` were private methods so I made those public (removing the `_`) and restarted ty but that didn't change anything. 

I tested with a couple more queries, with both `basedpyright` and `ty` and found the following:

- all `ty` matches were module level constants or class names; it didn't return any function names with the same string, or any function level variables (`basedpyright` did return all of those)
- the exception to the above is that it did find unit test functions with the target string
- when I created a new function during the same session with the target string, and then ran symbol search with `ty`, it did find the new function (but did not find pre-existing functions with that string); however it incorrectly returned _two_ matches: one in the file where the new function was created, and another match in another open file (but that was incorrect, that function was not called there). 

I tested with another python project, and found the same errors as above (ty only returned module level constants or class names), however, I found that it did return one function that had a decorator. 

I tested on a third python project, and it had the same error as reported above -- only showed module level constants and variables, not functions. See screenshot below.  

<img width="686" height="452" alt="Image" src="https://github.com/user-attachments/assets/a7e9d4bf-f878-4545-b7a4-0949fc17e16a" />

this is after restarting Zed and the ty language server, zed logs show no errors, only related logs are:
```
2025-12-11T09:42:15-08:00 INFO  [lsp] starting language server process. binary path: "/home/sean/.local/share/zed/languages/ty/ty-0.0.1-alpha.33/ty-x86_64-unknown-linux-gnu/ty", working directory: "/home/sean/dev/pd", args: ["server"]
2025-12-11T09:42:16-08:00 INFO  [lsp] starting language server process. binary path: "/home/sean/.local/share/zed/languages/ruff/ruff-0.14.8/ruff-x86_64-unknown-linux-gnu/ruff", working directory: "/home/sean/dev/pd", args: ["server"]
```


### Version

0.0.1-alpha.33/ty-x86_64-unknown-linux-gnu

---

_Label `server` added by @AlexWaygood on 2025-12-11 17:46_

---

_Comment by @MichaReiser on 2025-12-11 18:03_

Thanks for reporting. 

> I tested on a third python project, and it had the same error as reported above -- only showed module level constants and variables, not functions. See screenshot below.

Are any of those public? That would be easiest for me to ensure I'm seeing the same

---

_Comment by @insane-dreamer on 2025-12-11 18:30_

@MichaReiser not public repos, but I pulled out a portion of the project into its own folder/project, and replicated the same issue, so this might be useful as a testbed. See screenshot with new search and partial matches

<img width="710" height="730" alt="Image" src="https://github.com/user-attachments/assets/9e9f19fe-a23e-44ce-85b1-80bb397735a5" />

[pdpart.zip](https://github.com/user-attachments/files/24109651/pdpart.zip)

---

_Comment by @MichaReiser on 2025-12-11 19:21_

I have a fix for it not returning nested workspace symbols but there's a second bug related to `__all__` that @BurntSushi might want to take a look at. 

It seems, that `SymbolVisitor` only returns symbols listed in `__all__` when it sees any `__all__` definition. This is the correct behavior for global symbols only but we should probably ignore `__all__` completely for workspace symbols. We should probably also double check if there's any other *global symbols* behavior that we added recently that shouldn't apply to workspace symbols.

---

_Assigned to @BurntSushi by @MichaReiser on 2025-12-11 19:21_

---

_Label `bug` added by @MichaReiser on 2025-12-11 19:21_

---

_Closed by @BurntSushi on 2025-12-12 12:29_

---
