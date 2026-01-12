```yaml
number: 8925
title: Docker configuration broken after latest 0.5.0 release
type: issue
state: closed
author: niels-bosman
labels:
  - question
assignees: []
created_at: 2024-11-08T08:26:15Z
updated_at: 2024-11-08T14:36:00Z
url: https://github.com/astral-sh/uv/issues/8925
synced_at: 2026-01-12T15:59:38Z
```

# Docker configuration broken after latest 0.5.0 release

---

_@niels-bosman_

Hey all, wanted to flag that our docker build system failed after the latest 0.5.0 release, this is the `Dockerfile`:

```Dockerfile
FROM python:3.12.5-slim

WORKDIR /app

RUN apt-get update && \
    apt-get install -y curl unzip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN curl -LsSf https://astral.sh/uv/install.sh | sh
ENV PATH="/root/.cargo/bin/:$PATH"

COPY requirements.txt .

COPY . /app

RUN /root/.cargo/bin/uv pip install --system --no-cache-dir -r requirements.txt

EXPOSE 7000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "7000", "--proxy-headers"]
```

We fixed it for now by pinning the uv version to v4:

```Dockerfile
FROM python:3.12.5-slim

WORKDIR /app

RUN apt-get update && \
    apt-get install -y curl unzip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN curl -LsSf https://astral.sh/uv/0.4.30/install.sh | sh
ENV PATH="/root/.cargo/bin/:$PATH"

COPY requirements.txt .

COPY . /app

RUN /root/.cargo/bin/uv pip install --system --no-cache-dir -r requirements.txt

EXPOSE 7000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "7000", "--proxy-headers"]
```

What is the recommended way forward for v5 compatibility?

---

_Comment by @my1e5 on 2024-11-08 08:47_

The uv binary isn't installed in `.cargo/bin` anymore. Check the release notes for full details on this - https://github.com/astral-sh/uv/releases/tag/0.5.0 . But I think simply changing to `.local/bin` will fix this for you.

---

_Label `question` added by @konstin on 2024-11-08 11:14_

---

_Closed by @zanieb on 2024-11-08 14:36_

---
