---
number: 2027
title: "Can't use django binary that is located in the virtual environment"
type: issue
state: closed
author: ZeyadMoustafaKamal
labels:
  - question
assignees: []
created_at: 2024-02-27T22:26:08Z
updated_at: 2024-03-03T23:24:39Z
url: https://github.com/astral-sh/uv/issues/2027
synced_at: 2026-01-10T01:23:10Z
---

# Can't use django binary that is located in the virtual environment

---

_Issue opened by @ZeyadMoustafaKamal on 2024-02-27 22:26_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

so I created a virtual environment, sourced it, and installed Django. the version that is installed globally is `4.2.10` but the version I installed in the virtual environment is the latest one `5.0.2` and this is what I want to share with you
```
(python-tests) ➜  python-tests  uv pip install django
⠦ Resolving dependencies...                                                                                  Resolved 4 packages in 10.64s
Installed 4 packages in 812ms
 + asgiref==3.7.2
 + django==5.0.2
 + sqlparse==0.4.4
 + typing-extensions==4.10.0
(python-tests) ➜  python-tests  django-admin --version
4.2.10
```

and when I typed `django-admin startproject myproject .` it used django `4.2.10` not `5.0.2`
this is uv version
```
uv --version
uv 0.1.11
```
What I think might be the problem is the way you are creating the virtual environment. as when I use `pip` it uses the global pip not the one in the virtual environment. 

What is think about the solution is to provide a way like `uv exec some-command` like other tools that will use the binaries in the virtual environment, not the global ones.

And in the end, I am sorry that I can't help to solve this problem as the only thing I know about rust is how to type `Hello world` 🙂

---

_Comment by @charliermarsh on 2024-02-27 22:44_

Hey there. `uv` always installs into the virtual environment -- have you tried activating the virtual environment before running `django-admin`, with something like `source .venv/bin/activate`?

---

_Label `question` added by @charliermarsh on 2024-02-27 22:44_

---

_Comment by @ZeyadMoustafaKamal on 2024-02-27 23:34_

Yes I activated it.

and btw I know that uv installs the packages into the virtual environment and it installs Django successfully. but the problem is when I execute the django binary `django-admin` (it's like `uv` binary we get when we install it using pip) even if I sourced the virtual environment `source .venv/bin/activate` and then runs `django-admin` the actual `django-admin` that runs is the global one that is installed outside the virtual environment.

and to give you a better prespective run these commands so you can know what I am facing

```
pip install "django==4.2.10" # install django "4.2.10" globally
uv venv && source .venv/bin/activate && uv pip install "django==5.0.2"

django-admin --version
```
the output of the last command should be "5.0.2" but what I am getting is "4.2.10"

---

_Comment by @ZeyadMoustafaKamal on 2024-02-28 02:10_

@charliermarsh Something else I want you to keep in mind. if I created a virtual environment, activated it, and then ran `pip freeze` I would get the packages that are installed globally. which as I said earlier might be related to the way you are creating the virtual environment.

---

_Comment by @zanieb on 2024-02-28 02:55_

>  if I created a virtual environment, activated it, and then ran pip freeze I would get the packages that are installed globally. which as I said earlier might be related to the way you are creating the virtual environment.

We don't install `pip` in virtual environments by default, so you're probably getting the `pip` from your global installation. What does `uv pip freeze` show? What happens if you do `uv pip install pip` then `pip freeze`?

After activating the virtual environment, is its `bin` folder earlier than your global Python in the `PATH` variable? Is there a `django-admin` binary present in `.venv/bin/`?

---

_Comment by @zanieb on 2024-02-28 02:56_

Also the feature you requested is being tracked in #1207 

---

_Comment by @ZeyadMoustafaKamal on 2024-02-28 12:29_

@zanieb if I installed pip using uv, `pip freeze` will still give me all packages installed globally. But not a problem as `uv pip freeze` works in the virtual environment it will be fine.

and yes this `bin` of the virtual environment is earlier than the global python path. This is the `PATH` variable if you want to take a look

```
/tmp/python-tests/.venv/bin:/home/zeyad/.bun/bin:/home/zeyad/.local/bin:/home/zeyad/.modular/pkg/packages.modular.com_mojo/bin:/home/zeyad/.cargo/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/wsl/lib:/mnt/c/Program Files (x86)/Common Files/Oracle/Java/javapath:/mnt/c/Windows/system32:/mnt/c/Windows:/mnt/c/Windows/System32/Wbem:/mnt/c/Windows/System32/WindowsPowerShell/v1.0/:/mnt/c/Windows/System32/OpenSSH/:/mnt/c/Program Files (x86)/NVIDIA Corporation/PhysX/Common:/mnt/c/Program Files/NVIDIA Corporation/NVIDIA NvDLISR:/mnt/c/WINDOWS/system32:/mnt/c/WINDOWS:/mnt/c/WINDOWS/System32/Wbem:/mnt/c/WINDOWS/System32/WindowsPowerShell/v1.0/:/mnt/c/WINDOWS/System32/OpenSSH/:/mnt/c/Program Files/Git/cmd:/mnt/c/Program Files/nodejs/:/mnt/c/Users/Moaz-Mostafa/AppData/Local/Programs/Python/Python311/Scripts:/mnt/c/Program Files/Microsoft VS Code/bin/code:/mnt/c/Users/Moaz-Mostafa/AppData/Local/Microsoft/WindowsApps:/mnt/c/ProgramData/chocolatey/bin:/Docker/host/bin:/mnt/c/Users/Moaz-Mostafa/AppData/Local/Programs/Python/Launcher/:/mnt/c/Users/Moaz-Mostafa/.cargo/bin:/mnt/c/Users/Moaz-Mostafa/AppData/Local/Programs/Microsoft VS Code/bin:/mnt/d/Temp:/mnt/c/Users/Moaz-Mostafa/AppData/Local/Programs/oh-my-posh/bin:/mnt/c/Users/Moaz-Mostafa/AppData/Local/Programs/Hyper/resources/bin:/mnt/c/tools/neovim/nvim-win64/bin:/home/zeyad/Android/Sdk/tools:/home/zeyad/Android/Sdk/platform-tools#:/home/zeyad/.fzf/bin
```

the first one is the `bin` directory of my virtual environment.

and `django-admin` is also present in the `bin` directory which is strange btw. it should be executed correctly. What it might be related to??

can you please try to do this yourself. I am using `WSL`, so this might be the problem.

just run these commands and see what is going on

```
pip install "django==4.2.10" # install django "4.2.10" globally
uv venv && source .venv/bin/activate && uv pip install "django==5.0.2"

django-admin --version
```

what the last line gives you?? `4.2.10` or `5.0.2` ??

---

_Comment by @konstin on 2024-02-28 12:54_

Unfortunately i can't reproduce the problem on WSL:

```
~/gh2027$ pip install "django==4.2.10" # install django "4.2.10" globally
Defaulting to user installation because normal site-packages is not writeable
Collecting django==4.2.10
  Using cached Django-4.2.10-py3-none-any.whl (8.0 MB)
Collecting asgiref<4,>=3.6.0
  Using cached asgiref-3.7.2-py3-none-any.whl (24 kB)
Collecting sqlparse>=0.3.1
  Using cached sqlparse-0.4.4-py3-none-any.whl (41 kB)
Collecting typing-extensions>=4
  Using cached typing_extensions-4.10.0-py3-none-any.whl (33 kB)
Installing collected packages: typing-extensions, sqlparse, asgiref, django
Successfully installed asgiref-3.7.2 django-4.2.10 sqlparse-0.4.4 typing-extensions-4.10.0
~/gh2027$ uv venv && source .venv/bin/activate && uv pip install "django==5.0.2"
Using Python 3.10.12 interpreter at /usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
Resolved 4 packages in 2ms
Installed 4 packages in 174ms
 + asgiref==3.7.2
 + django==5.0.2
 + sqlparse==0.4.4
 + typing-extensions==4.10.0
(gh2027) ~/gh2027$ django-admin --version
5.0.2
```

 * `python --version`: 3.10.2
 * `python -c "import sys; print(sys.path)"`: `['', '/usr/lib/python310.zip', '/usr/lib/python3.10', '/usr/lib/python3.10/lib-dynload', '/home/konsti/gh2027/.venv/lib/python3.10/site-packages']`
 * OS: Ubuntu 22.04.4
 * `python -m sysconfig`:
```
Platform: "linux-x86_64"
Python version: "3.10"
Current installation scheme: "posix_prefix"

Paths:
        data = "/home/konsti/gh2027/.venv"
        include = "/usr/include/python3.10"
        platinclude = "/usr/include/python3.10"
        platlib = "/home/konsti/gh2027/.venv/lib/python3.10/site-packages"
        platstdlib = "/home/konsti/gh2027/.venv/lib/python3.10"
        purelib = "/home/konsti/gh2027/.venv/lib/python3.10/site-packages"
        scripts = "/home/konsti/gh2027/.venv/bin"
        stdlib = "/usr/lib/python3.10"`
```

`PATH` starts with `/home/konsti/gh2027/.venv/bin:` and `VIRTUAL_ENV` is `/home/konsti/gh2027/.venv`.

How did you install python, and what do `python -c "import sys; print(sys.path)"` and `python -m sysconfig` say for you?

---

_Comment by @ZeyadMoustafaKamal on 2024-02-28 14:21_

@konstin I can't remember how I installed Python but I think it was using the `apt` package manager

when I activate the virtual environment the `python -c "import sys; print(sys.path)"` gives me `['', '/usr/lib/python310.zip', '/usr/lib/python3.10', '/usr/lib/python3.10/lib-dynload', '/tmp/python-tests/.venv/lib/python3.10/site-packages']`

and the `python -m sysconfig` gives me 
```Paths:
        data = "/tmp/python-tests/.venv"
        include = "/usr/include/python3.10"
        platinclude = "/usr/include/python3.10"
        platlib = "/tmp/python-tests/.venv/lib/python3.10/site-packages"
        platstdlib = "/tmp/python-tests/.venv/lib/python3.10"
        purelib = "/tmp/python-tests/.venv/lib/python3.10/site-packages"
        scripts = "/tmp/python-tests/.venv/bin"
        stdlib = "/usr/lib/python3.10"
```
this is strange btw if you look at the scripts you will notice that it uses the `bin` of the virtual environment which should make be able to use `django-admin` without any problem. I will ask you to see this and tell me if I did something wrong.[![asciicast](https://asciinema.org/a/Px6IRN6VDRgyBAVQpsSoerAph.svg)](https://asciinema.org/a/Px6IRN6VDRgyBAVQpsSoerAph)

---

_Comment by @konstin on 2024-02-28 14:46_

Can you try cleaning the uv cache (`uv clean`) and after reinstalling, try running `.venv/bin/django-admin` directly?

---

_Comment by @ZeyadMoustafaKamal on 2024-02-28 15:25_

@konstin I tried to clear the cache but I still have the same problem. I tried to use `.venv/bin/django-admin` and it worked successfully but the problem I am afraid of is this may affect some other components of `uv`.

And btw I tried to delete uv bin from `~/.cargo/bin/uv` and re-install it but I still had the same problem. I will use `.venv/bin/django-admin` but I hope that this be fixed in some way. I am going to leave this issue opened but if you want to close it that's ok.

---

_Comment by @ZeyadMoustafaKamal on 2024-02-28 23:31_

@konstin I just want to ask you about something. when I activate the virtual environment, I get this `(python-tests) ➜  python-tests` shouldn't it be like `(.venv) ➜  python-tests` or you have changed it??

And btw I tried to run `uv` on my Windows machine and everything worked ok so I believe that there is something wrong with my `uv`. As I said earlier I removed `uv` from `~/.cargo/bin/uv` and installed it again. Should I delete something else so I can make sure that I have completely uninstalled `uv` from my WSL?

---

_Comment by @ZeyadMoustafaKamal on 2024-02-29 00:09_

@konstin I am sorry for calling you several times but I just finally fixed the problem and I have no idea how it was fixed.

in the `.venv/bin/activate` in line 80 I saw `VIRTUAL_ENV_PROMPT="dummy"` as `dummy` is the parent directory of the virtual environment. and I just changed `dummy` to `.venv` and then deactivated the virtual environment and activated it again and everything worked just fine. I just hope to know why it gives me "dummy" when it should give me ".venv" automatically??

See this if you want [![asciicast](https://asciinema.org/a/sj9PZ6c9kkNaRRVSwIkUisH5q.svg)](https://asciinema.org/a/sj9PZ6c9kkNaRRVSwIkUisH5q)

---

_Comment by @konstin on 2024-02-29 09:04_

Sorry, no idea why this makes a difference

---

_Comment by @ZeyadMoustafaKamal on 2024-02-29 15:49_

I have no idea also. But the question is do you know why it's "dummy" not ".venv" ?? if we just solved this, then everything will be OK.

---

_Comment by @zanieb on 2024-03-02 14:51_

The prompt should only be cosmetic i.e. it's just the text that's displayed to identify the virtual environment. I also could not reproduce this on macOS — I'm not sure what's going on but it does seem specific to something in your environment. If there's something that relies on `VIRTUAL_ENV_PROMPT` to determine binaries in your path, that sounds like a bug with their software.

---

_Comment by @zanieb on 2024-03-02 17:39_

I'm going to close this so we can keep the issue tracker actionable, thanks for all your investigation let us know if you find out more.

#1570 has more details on the default prompt value.

---

_Closed by @zanieb on 2024-03-02 17:39_

---

_Comment by @ZeyadMoustafaKamal on 2024-03-03 23:24_

For anyone having the same problem I just noticed that the problem isn't with the `VIRTUAL_ENV_PROMPT` it's all that after installing `Django` you will just have to deactivate and activate the virtual environment again.

---
