---
number: 10557
title: "uv self update causes ''GitHub API rate limit exceeded.\" error"
type: issue
state: closed
author: AmineDjeghri
labels:
  - question
assignees: []
created_at: 2025-01-13T08:20:25Z
updated_at: 2025-04-25T12:10:59Z
url: https://github.com/astral-sh/uv/issues/10557
synced_at: 2026-01-07T13:12:18-06:00
---

# uv self update causes ''GitHub API rate limit exceeded." error

---

_Issue opened by @AmineDjeghri on 2025-01-13 08:20_

A preview of the error 
![Image](https://github.com/user-attachments/assets/32718b3c-7479-4314-83ea-5e66c6572e26)

Edit : more information 

- UV version : 0.5.17
- Operating System : MacOS 14
- Command and output :  check the image



---

_Comment by @nbaju1 on 2025-01-13 08:33_

[Before posting in the issue tracker](https://github.com/astral-sh/uv/issues/9452)

---

_Comment by @AmineDjeghri on 2025-01-13 08:40_

@nbaju1 Thanks. I updated the description of the issue. 

---

_Comment by @AmineDjeghri on 2025-01-13 08:41_

Update : now for example. It works 

<img width="821" alt="Image" src="https://github.com/user-attachments/assets/675b7967-8e96-46a0-aabf-99ad9d86235a" />

---

_Comment by @cb-robot on 2025-01-13 09:37_

GitHub rate limits are based on the user making the requests (in this case, you), not on the project being accessed. I'm guessing uv is implementing `self update` by hitting the GitHub API, and you've just run it too many times in a short period of time or are running other things that are hitting the API from your IP address.

The [limit for unauthenticated users](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api?apiVersion=2022-11-28) is 60 API requests per hour. The limit for authenticated users is massively higher, 5000 per hour. So the only thing `uv` could really do here would be to allow you to specify a GitHub API token for it to use when accessing the GitHub API. 

---

_Comment by @blueraft on 2025-01-13 09:43_

You can specify a token using `--token` option.

```console
❯ uv self update --help
...
Options:
      --token <TOKEN>  A GitHub token for authentication. A token is not required but can be used to reduce the chance of encountering rate limits [env: UV_GITHUB_TOKEN=]
```

---

_Closed by @zanieb on 2025-01-13 18:42_

---

_Label `question` added by @zanieb on 2025-01-13 18:42_

---

_Comment by @zorion on 2025-04-25 09:52_



> GitHub rate limits are based on the user making the requests (in this case, you), not on the project being accessed. I'm guessing uv is implementing `self update` by hitting the GitHub API, and you've just run it too many times in a short period of time or are running other things that are hitting the API from your IP address.
> 
> The [limit for unauthenticated users](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api?apiVersion=2022-11-28) is 60 API requests per hour. The limit for authenticated users is massively higher, 5000 per hour. So the only thing `uv` could really do here would be to allow you to specify a GitHub API token for it to use when accessing the GitHub API.

I ran `uv self update` on March, the 31st. (so my version is v0.6.11)

The first time I ran it today I received the "rate limit".

I'm not aware of having sent another 59 requests to github today, perhaps I have some software doing it for me, otherwise, I wonder how many requests perform `uv self update`...

The following image doesn't show my "first" attempt (after 25 days) but some retries.

<img width="587" alt="Image" src="https://github.com/user-attachments/assets/234c3ad2-ff0b-445b-a3f6-7621fffaff85" />

I may try the "TOKEN" approach

---

_Comment by @zorion on 2025-04-25 10:00_

<img width="306" alt="Image" src="https://github.com/user-attachments/assets/8d3fcc94-f240-41b6-8f11-8edf8796992e" />

It did work, I set the token expiration to 7 days so I will likely create another one next time I want to update.

I wonder where those 60 requests come from, but it may be a question for GitHub (I'm under CGNAT, may other users with my share my anonymous limit?)

---

_Comment by @ceejatec on 2025-04-25 11:21_

@zorion Yeah, if you're anonymous, Github rate limits based on your IP since that's the only information it has. So if others use the same external IP, that can throttle your connections too.

---

_Comment by @zorion on 2025-04-25 12:08_

> [@zorion](https://github.com/zorion) Yeah, if you're anonymous, Github rate limits based on your IP since that's the only information it has. So if others use the same external IP, that can throttle your connections too.

Sheeet, it was not my CGNAT! it was my Cloudflare-Warp (VPN like software)! Silly me!

Thanks @ceejatec  ! You are right!

Checked with `curl https://api.github.com/rate_limit 2> /dev/null | jq ".rate"`
With the VPN:
```json
{
  "limit": 60,
  "remaining": 30,
  "reset": 1745585596,
  "used": 30,
  "resource": "core"
}

```

Without:
```json
{
  "limit": 60,
  "remaining": 60,
  "reset": 1745586409,
  "used": 0,
  "resource": "core"
}
```

---
