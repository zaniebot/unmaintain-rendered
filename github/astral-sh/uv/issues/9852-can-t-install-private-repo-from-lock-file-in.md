---
number: 9852
title: "Can't install private repo from lock file in docker (works in pip and poetry just fine and uv sync seems to create the uv.lock file just fine"
type: issue
state: closed
author: adiberk
labels: []
assignees: []
created_at: 2024-12-12T21:24:17Z
updated_at: 2024-12-12T22:39:11Z
url: https://github.com/astral-sh/uv/issues/9852
synced_at: 2026-01-07T13:12:18-06:00
---

# Can't install private repo from lock file in docker (works in pip and poetry just fine and uv sync seems to create the uv.lock file just fine

---

_Issue opened by @adiberk on 2024-12-12 21:24_

We have a private repo that we use in our app
I have it formatted in pyproject.toml as such
```
dependencies = [
.....
    "{name}",
]
[tool.uv.sources]
{name} = { git = "ssh://git@github.com/.../{package_name}.git", rev = "2.3.4" }
```
Here is how it is formatted in requirements.txt file
`{name}@git+ssh://git@github.com/.../{package_name}.git@2.3.4`
And here is working format in poetry pyproject
```
[tool.poetry.dependencies]
python = "3.11.4",
{name} = { git = "ssh://git@github.com/.../{package.name}.git", rev = "2.3.4", extras = [
...
] }
```


I am then installing the project from the lockfile using our typical git ssh configuration process in docker (works in pip and poetry) and am left with an error via uv
```
 Caused by: Failed to fetch wheel: {name}} @ git+ssh://git@github.com/.../{package_name}.git@277c69b6e88a8....
```

I do have an idea as to why but cant be sure - It seems like the "rev" number isn't showing up in this error (it is using some other hash?). This tells me it isn't using a specific version (which I am listing in the pyproject.toml). I am not positive but maybe this is a result of how we use and tag releases?

If I do a uv export --no-hashes
`{name} @ git+ssh://git@github.com/.../{package_name}.git@277c69b6....`
Which is missing the rev I provided and seems to be using some other hash

Either way, it seems to work find in pip and poetry and could use some help figuring out how to resolve this
(command being run in dockerfile `uv sync --frozen --no-install-project`)


Some info on my dockerfile
```
OPY --from=ghcr.io/astral-sh/uv:0.5.8 /uv /bin/uv

WORKDIR /app

RUN uv venv --relocatable

ENV LIBRARY_PATH=/lib:/usr/lib
ENV PATH="/app/.venv/bin:$PATH"
ENV UV_PYTHON_INSTALL_DIR=/opt/uv/python

COPY pyproject.toml uv.lock ./
# add github.com to known hosts
.... ssh-keyscan github.com >> ~/.ssh/known_hosts
RUN echo "Environment: ${ENV}"
RUN --mount=type=secret..... \
    if [ "$ENV" = "unittest" ]; then \
    echo 'Installing UT' && uv sync --extra ut --frozen --no-install-project; \
    elif [ "$ENV" = "dev" ]; then \
    echo 'Installing DEV' && uv sync --extra dev --frozen --no-install-project; \
    else \
    echo 'Installing REG' && uv sync --frozen --no-install-project; \
    fi
````








---

_Renamed from "Can't install private repo from lock file in docker (works in poetry just fine)" to "Can't install private repo from lock file in docker (works in pip and poetry just fine and uv sync works fine on my local machine)" by @adiberk on 2024-12-12 21:40_

---

_Renamed from "Can't install private repo from lock file in docker (works in pip and poetry just fine and uv sync works fine on my local machine)" to "Can't install private repo from lock file in docker (works in pip and poetry just fine and uv sync seems to create the uv.lock file just fine" by @adiberk on 2024-12-12 22:10_

---

_Comment by @adiberk on 2024-12-12 22:38_

I am a bit of an idiot and my mount was incorrect (for what I was doing locally with my docker image)

---

_Closed by @adiberk on 2024-12-12 22:38_

---
