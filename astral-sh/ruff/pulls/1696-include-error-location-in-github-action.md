```yaml
number: 1696
title: Include error location in GitHub Action diagnostic messages
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/actions
created_at: 2023-01-06T17:12:50Z
updated_at: 2023-01-06T20:58:17Z
url: https://github.com/astral-sh/ruff/pull/1696
synced_at: 2026-01-12T15:55:06Z
```

# Include error location in GitHub Action diagnostic messages

---

_@charliermarsh_

This ensures that if you look at the GitHub Actions _logs_, you see the entire message, including the location:

![Screen Shot 2023-01-06 at 3 08 06 PM](https://user-images.githubusercontent.com/1309177/211091772-df6f1deb-c741-435c-be2e-6ee22347a073.png)

The downside is that the location gets repeated inline:

![Screen Shot 2023-01-06 at 3 08 30 PM](https://user-images.githubusercontent.com/1309177/211091800-57020736-95fa-4e41-acb3-eb11c848ba7e.png)

See: #1693.

---

_@saadmk11 reviewed on 2023-01-06 17:24_

---

_Review comment by @saadmk11 on `src/printer.rs`:230 on 2023-01-06 17:24_

GitHub Actions Only uses these options for annotation. You probably need to add the file path to the message `::({})`

---

_Review comment by @charliermarsh on `src/printer.rs`:230 on 2023-01-06 17:26_

What I'm trying to resolve is that without this change, in the CI logs, you _only_ see the message:

<img width="544" alt="Screen Shot 2023-01-06 at 12 25 36 PM" src="https://user-images.githubusercontent.com/1309177/211064954-f35aaec7-4087-4e03-9919-eb123ff514a5.png">

What I'd like is to surface the annotations, but also show the correct info in the logs. Any ideas?

---

_@charliermarsh reviewed on 2023-01-06 17:26_

---

_@saadmk11 reviewed on 2023-01-06 17:27_

---

_Review comment by @saadmk11 on `src/printer.rs`:230 on 2023-01-06 17:27_

Ref: https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions#setting-a-notice-message

---

_@saadmk11 reviewed on 2023-01-06 17:31_

---

_Review comment by @saadmk11 on `src/printer.rs`:230 on 2023-01-06 17:31_

Probably replacing `message.kind.body(),` with  something like this will work: `format!("{}: {}", relativize_path(Path::new(&message.filename)), message.kind.body())`

---

_@saadmk11 reviewed on 2023-01-06 17:34_

---

_Review comment by @saadmk11 on `src/printer.rs`:230 on 2023-01-06 17:34_

Or separately printing the file names without using the options will work

---

_Review comment by @saadmk11 on `src/printer.rs`:251 on 2023-01-06 17:42_

Does something like this work?

```python
diagnostics.messages.iter().for_each(|message| {
      print!(
          "::error title=Ruff ({}),file={},line={},col={},endLine={},\
           endColumn={}::{}",
          message.kind.code(),
          relativize_path(Path::new(&message.filename)),
          message.location.row(),
          message.location.column(),
          message.end_location.row(),
          message.end_location.column(),
          message.kind.body(),
      );
      print!(" [{}:{}]", relativize_path(Path::new(&message.filename)), message.location.row());
  });
```
Notice: use of `print!` macro

---

_@saadmk11 reviewed on 2023-01-06 17:42_

---

_Comment by @charliermarsh on 2023-01-06 20:08_

A few options...

First, print info like this (too noisy, I think): 

<img width="884" alt="Screen Shot 2023-01-06 at 12 22 00 PM" src="https://user-images.githubusercontent.com/1309177/211065613-a51a0b26-f8b0-4689-83e5-a446459e4386.png">

Second, interleave the readable and notice info:

<img width="930" alt="Screen Shot 2023-01-06 at 12 29 03 PM" src="https://user-images.githubusercontent.com/1309177/211065662-6a5a2686-8a4e-40ea-bf0b-037cff5e59b2.png">

Third, is to print out all the readable messages, then all of the diagnostics for GitHub:

![Screen Shot 2023-01-06 at 1 45 16 PM](https://user-images.githubusercontent.com/1309177/211078331-d4ea22af-f8f4-440f-84f3-4b867d5cabef.png)

Fourth would be to print the filenames as part of the message:

![Screen Shot 2023-01-06 at 3 08 06 PM](https://user-images.githubusercontent.com/1309177/211091199-dda8e97a-119d-49ab-a1c7-8acdf2d6c309.png)

I think I prefer the last option, but it does mean the filename is repeated in the actual annotation:

![Screen Shot 2023-01-06 at 3 08 30 PM](https://user-images.githubusercontent.com/1309177/211091264-9aae9e17-97e9-41b8-9f59-6b8e0f31423c.png)


---

_Comment by @charliermarsh on 2023-01-06 20:08_

\cc @edgarrmondragon - in case you have an opinion here.

---

_Renamed from "Debug GitHub Actions reporter" to "Include error location in GitHub Action diagnostic messages" by @charliermarsh on 2023-01-06 20:10_

---

_Merged by @charliermarsh on 2023-01-06 20:58_

---

_Closed by @charliermarsh on 2023-01-06 20:58_

---

_Branch deleted on 2023-01-06 20:58_

---
