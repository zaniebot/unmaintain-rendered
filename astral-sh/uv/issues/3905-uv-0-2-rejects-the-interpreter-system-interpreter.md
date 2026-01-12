```yaml
number: 3905
title: uv 0.2 rejects the interpreter, system interpreter not allowed
type: issue
state: closed
author: morotti
labels:
  - needs-decision
assignees: []
created_at: 2024-05-29T14:54:14Z
updated_at: 2024-06-12T13:56:34Z
url: https://github.com/astral-sh/uv/issues/3905
synced_at: 2026-01-12T15:58:46Z
```

# uv 0.2 rejects the interpreter, system interpreter not allowed

---

_@morotti_

Hello,

I've upgraded uv and I am finding that it cannot run anymore. 
It's finding the python interpeter but refusing to use it.

Debug logs:

```
[root@f2652ef0cc07 default-venv]# python --version
Python 3.8.12

[root@f2652ef0cc07 default-venv]# uv --version
uv 0.2.5

[root@f2652ef0cc07 default-venv]# uv pip install --dry-run mypackage -vvv
 uv_requirements::specification::from_source source=mypackage
    0.003133s DEBUG uv_interpreter::discovery Searching for Python interpreter in virtual environments
    0.003453s DEBUG uv_interpreter::discovery Found CPython 3.8.12 at `/default-venv/bin/python3` (active virtual environment)
    0.003474s DEBUG uv_interpreter::discovery Ignoring Python interpreter at `/default-venv/bin/python3`: system interpreter not allowed
error: No Python interpreters found in virtual environments

[root@f2652ef0cc07 default-venv]# echo $PATH
/default-venv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

[root@f2652ef0cc07 default-venv]# echo $VIRTUAL_ENV
/default-venv

[root@f2652ef0cc07 default-venv]# which -a python
/default-venv/bin/python
/usr/bin/python
/bin/python

[root@f2652ef0cc07 default-venv]# which -a python3
/default-venv/bin/python3
```

For context, this is running in a centos container. `/default-venv` is where we put the venv in our containers.

For reference, we built and package the python interpreter and prepare the venv ourselves. There's a possibility that doesn't match your expectations of a venv.

looking at the logs:
uv is finding the venv -> "0.003453s DEBUG uv_interpreter::discovery Found CPython 3.8.12 at `/default-venv/bin/python3` (active virtual environment)
but it's just refusing to use it, thinking it's a system interpreter, how come? -> "Ignoring Python interpreter at `/default-venv/bin/python3`: system interpreter not allowed"
"

The only characteristics of a venv as far as I know, and as setup by the official ".../bin/activate" script, is to set the `$VIRTUAL_ENV` environment variable to the directory of the venv and set `$PATH` to the "<venv>/bin" directory. 
Both variables are set appropriately as far as I can tell. The venv is legit. 
`which -a` is highlighting other paths to other python interpreters including the actual system interpreter.

What does uv think my venv is a not a venv? 🤔 

P.S. I figured I can pass "uv pip install --python $VIRTUAL_ENV/bin/python3" to workaround the issue. 

Regards.

---

_Comment by @charliermarsh on 2024-05-29 14:56_

A virtualenv definitionally (at least in pip) is an environment in which `sys.prefix != sys.base_prefix`. Does your environment have that quality?

---

_Comment by @morotti on 2024-05-29 15:14_

wow that's a really quick reply, thank you

I am checking and the venv has sys.prefix == sys.base_prefix.
I think it might be looking more like a build of the interpreter, rather than a separate venv.
but it's still not the system interpreter.

Interestingly the definition of pip appears to be different than the definition in the interpreter.
You can find the official bin/activate script here to activate a venv https://github.com/python/cpython/blob/main/Lib/venv/scripts/common/activate
It's just setting `$VIRTUAL_ENV` and `$PATH`, that is what defines when running in a virtual environment as far as I know. 

I wonder if `$VIRTUAL_ENV` should be checked instead of or in addition to `sys.prefix != sys.base_prefix`. 🤔 


I see a few other tickets were people are reporting uv stopped working in containers (in official python containers, github actions, etc...), same issue where it refuses to use the system interpreter, but the interpreter might actually be the system interpreter in the container. It would be worthwhile to check how these are setup and what environment variables they have for comparison. 


---

_Comment by @charliermarsh on 2024-05-29 15:38_

Yeah, people used to use `$VIRTUAL_ENV` to point to an arbitrary Python, but we deprecated that usage -- we now require that it's a formal virtual environment, and suggest `--python` for folks that are installing into non-virtual Python environments.

Your case is a bit different, because you do consider this virtual, but it doesn't match the definition we share with pip. I'm not sure how to handle 🤔 

---

_Label `needs-decision` added by @charliermarsh on 2024-05-29 15:38_

---

_Comment by @zanieb on 2024-05-29 20:54_

I'm happy to look into further this when I'm back from vacation next week.

Are you missing the `pyvenv.cfg` file? I believe this is required by the specification in [PEP 405](https://peps.python.org/pep-0405/#specification) and is what results in the differing `sys.prefix`.

We may want to be more lenient here, but I think so far we've been seeing usage of `VIRTUAL_ENV` that is not complaint with the specification and I would generally like to encourage people to follow the standard if feasible. If this turns out to be a significant burden, we can definitely explore relaxing our constraints or proposing updates to the Python standards.

---

_Comment by @morotti on 2024-06-03 14:56_

Hello, sorry for delay to get back,

I've checked and there is no `pyvenv.cfg` file. 
I think it's not a venv but a fresh build of the interpreter in a specific location. It's not a system interpreter either.
I can pass the flags to work around the issue.


---

_Comment by @zanieb on 2024-06-04 13:36_

No problem I was on vacation until today! :)

I think there's some difficulty in the language here since, to us, "system" currently means "an interpreter that is not a virtual environment". I don't think we have a consistent way to identify if the interpreter actually belongs to the system (there's `EXTERNALLY-MANAGED` markers but those are not widely used yet). The idea is that we do not want to mutate interpreter environments that are not virtual environments without opt-in since that's Python best practice. 

It sounds like using `--system` or creating a `pyvenv.cfg` file are your best options right now. I am also thinking that if `VIRTUAL_ENV` is set to a interpreter that is not a virtual environment we should respect it without opt-in and just display a warning.

---

_Assigned to @zanieb by @zanieb on 2024-06-04 13:37_

---

_Comment by @zanieb on 2024-06-12 13:56_

We're still considering #4018 but I think I'll close this there is a clear workaround and furthermore a simple way to properly declare that the environment should be treated as a virtual environment.

---

_Closed by @zanieb on 2024-06-12 13:56_

---
