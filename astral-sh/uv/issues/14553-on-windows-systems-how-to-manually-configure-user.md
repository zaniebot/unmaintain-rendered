```yaml
number: 14553
title: On Windows systems, how to manually configure user-level configuration files（uv.toml）
type: issue
state: closed
author: Aspirant-A
labels:
  - question
assignees: []
created_at: 2025-07-11T01:22:14Z
updated_at: 2025-07-12T12:00:12Z
url: https://github.com/astral-sh/uv/issues/14553
synced_at: 2026-01-12T16:01:51Z
```

# On Windows systems, how to manually configure user-level configuration files（uv.toml）

---

_@Aspirant-A_

### Question

Due to the slow download speed when using the `uv python install` command, I would like to configure the download connection. However, I found that there is no `uv.toml` file in the `C:\Users\29876\AppData\Roaming\uv` directory. Do I need to create it myself? If I manually configure it, do I need to add the `--config_file` parameter when using `uv python install`

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @Aspirant-A on 2025-07-11 01:22_

---

_Comment by @charliermarsh on 2025-07-11 01:42_

Yeah, that sounds right. You need to create it yourself at `%APPDATA%\uv\uv.toml`. If you do that, uv will pick it up automatically -- no need to provide `--config-file`.

https://docs.astral.sh/uv/concepts/configuration-files/

---

_Comment by @Aspirant-A on 2025-07-11 02:25_

Can I also add UV_INDEX_URL to the system variables to permanently change the download source？

---

_Comment by @charliermarsh on 2025-07-11 02:27_

Yes, that's an option. You can set that globally however you typically set environment variables. Or you can define it in the aforementioned `uv.toml`:

```toml
index-url = "..."
```

---

_Comment by @Aspirant-A on 2025-07-11 02:45_

I am a Chinese user. After completing the configuration, whenever I use the "uv python install" command, the following error always appears。

<img width="1460" height="258" alt="Image" src="https://github.com/user-attachments/assets/2b9eb7ed-0bf1-4d73-9b24-bbe39b3ce3a7" />

---

_Comment by @Aspirant-A on 2025-07-11 05:31_


I have a question. When you configure the index.url, do uv python install ... and uv add ... all download from the url you configured.

---

_Comment by @FishAlchemist on 2025-07-11 14:01_

> I am a Chinese user. After completing the configuration, whenever I use the "uv python install" command, the following error always appears。

The relevant solutions, as mentioned in https://github.com/astral-sh/uv/issues/14187, can be referenced.


---

_Closed by @charliermarsh on 2025-07-12 12:00_

---
