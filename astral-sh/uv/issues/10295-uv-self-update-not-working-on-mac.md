```yaml
number: 10295
title: UV Self Update Not Working on MAC
type: issue
state: closed
author: kb-cov
labels:
  - bug
  - releases
assignees: []
created_at: 2025-01-03T22:34:43Z
updated_at: 2025-01-04T08:30:54Z
url: https://github.com/astral-sh/uv/issues/10295
synced_at: 2026-01-12T16:00:10Z
```

# UV Self Update Not Working on MAC

---

_@kb-cov_

Hi,

I have been successfully using uv self update for many months but as of last week I have ben receiving the following error message...

➜  ~ uv self update
Found existing alias for "uv self update". You should use: "uvup"
info: Checking for updates...
error: error decoding response body
  Caused by: there are extra bytes after body has been decompressed

I am on MAC OS Seqouia 15.1.1 and using iTerm2 to run the update command.

Is this a known issue with uv or something that has changed with my local setup?

Thanks,
Kev

---

_Label `releases` added by @zanieb on 2025-01-03 23:10_

---

_Comment by @zanieb on 2025-01-03 23:11_

That's unusual, thanks for the report!

Can you try `RUST_LOG=trace uv self update`?

---

_Comment by @kb-cov on 2025-01-03 23:18_

Hi....here you go...the output from that command is in attached file...[rust_log.txt](https://github.com/user-attachments/files/18304593/rust_log.txt)


---

_Comment by @zanieb on 2025-01-03 23:54_

Unfortunately nothing stands out there. I've reached out to the `cargo-dist` team which maintains the library we use for the self-updates.

---

_Label `bug` added by @zanieb on 2025-01-03 23:54_

---

_Comment by @FishAlchemist on 2025-01-04 07:24_

@zanieb  Actually, the self-update of v0.5.12 is known to be broken.
https://github.com/astral-sh/uv/issues/10196#issuecomment-2563857968
I think their mistakes are the same.


---

_Comment by @kb-cov on 2025-01-04 08:30_

HI, thank you both for fast responses....as per Charlie's instruction in 10196 I have re-installed from Curl and all is good now...many thanks.

---

_Closed by @kb-cov on 2025-01-04 08:30_

---
