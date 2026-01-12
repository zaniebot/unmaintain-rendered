```yaml
number: 7898
title: "Allow for `uv` global instantiation"
type: issue
state: closed
author: krstp
labels: []
assignees: []
created_at: 2024-10-03T14:46:41Z
updated_at: 2024-12-09T14:55:49Z
url: https://github.com/astral-sh/uv/issues/7898
synced_at: 2026-01-12T15:59:17Z
```

# Allow for `uv` global instantiation

---

_@krstp_

I would like to see `uv` recognized for global virtual environments in a similar way to how `pyenv` works. Instead of focusing solely on in-project folder development, allow for global env instantiation.

In theory, this task should be accomplished by creating all virtual environments via:

`uv venv ~/.venvs/my_project_env`

To activate the environment:

`source ~/.venvs/my_project_env/bin/activate`

Alternatively, `uv` could add the following function to `.bashrc/.zshrc`:

```
function uvactivate() {
    source ~/.venvs/$1/bin/activate
}
```

Then users could activate an environment like this:

`uvactivate my_project_env`

---

_Comment by @zanieb on 2024-10-03 14:55_

Related 

- #1495 
- https://github.com/astral-sh/uv/issues/1910

It seems like this is doable today with some small wrappers around uv, I'm not sure we'll add first-class support for this.

---

_Comment by @krstp on 2024-10-26 03:55_

The solution above works perfectly and seems to be a straightforward replacement for pyenv.

Here’s a bit more detail about my setup:
- My virtual environments are located at: `/Users/<user>/.config/uv/venv`.
- I started creating named virtual environments, such as `.uvcore12`, `.uvcore13`, etc. Since `uv` doesn’t support named environments directly, I follow this process:
    - I run `uv venv --python 3.xx`, which creates a folder named `.venv`.
    - I then rename `.venv` to my preferred name, like `.uvcore12`.
    - To ensure the prompt reflects the new name, I modify the `activate` script in the renamed folder:
        - Set `VIRTUAL_ENV=/Users/<user>/.config/uv/venv/.uvcore12`
        - Set `VIRTUAL_ENV_PROMPT="(core12)"`

Additionally, I added the following function to my `.bashrc` or `.zshrc`:

```bash
function uvactivate() {
    source ~/.config/uv/venv/.uv$1/bin/activate
}
```

With this setup, I can activate my environment/s from anywhere by simply calling `uvactivate core12`, and the prompt will show `3.12.6 (core12)`.

Then any `uv pip install ...` will install the new packages in the activated environment that works globally. I hope this helps.

Would be nice to have the `activate` command under `uv activate <env_name>`

---

_Comment by @CharlesPerrotMinot on 2024-10-26 04:40_

On our end, we simply do
`ENV UV_PROJECT_ENVIRONMENT="/usr/local/"`

---

_Comment by @krstp on 2024-10-26 13:19_

@CharlesPerrotMinotHCHB: with your proposed way, under `/usr/local/`, you will:
- keep overwriting the default python installation, as it always creates `.venv`,
- not be allowed to use named env instances
- miss on prompt env naming
- be unable to easily activate a diff named env.

Instead, for anything you do, you will always dip into `.venv`. The way I propose delivers all this functionality.

---

_Comment by @CharlesPerrotMinot on 2024-10-28 05:25_

@krstp true, but we're running in docker container.
Use case may vary, but for us, it's totally sufficient, and none of the things you say are an issue.

And to me, it makes sense for a global instantiation to be overwritten

---

_Comment by @krstp on 2024-11-03 12:50_

Yeah, for the docker it makes sense. What I am and was aiming for with global setup/instantiation is local env development. If it is only in docker, I agree it does not make sense, but in local imho it does shine and what I propose is an easy fix.

---

_Comment by @krstp on 2024-11-03 13:04_

For the reference to the issue, here seems to be a related tool that somewhat accomplishes similar functionality: https://github.com/AndydeCleyre/zpy

---

_Comment by @zanieb on 2024-11-03 14:30_

I think we can track this in #1495.

---

_Closed by @zanieb on 2024-11-03 14:30_

---

_Comment by @krstp on 2024-12-08 05:46_

Just for reference, where this really shines is when using code editors for specifying a default release. Currently, user needs to do its own mumbo-jumbo (as specified above) to have such environment. AFAIK most of editors do not allow for in-project venv read.

---

_Comment by @zanieb on 2024-12-09 14:55_

> AFAIK most of editors do not allow for in-project venv read

I don't think this is true, e.g., VSCodes _does_ discover virtual environments in the project directory. What are you referring to here?

---
