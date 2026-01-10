```yaml
number: 3568
title: ruff check file.py output is unreadable
type: issue
state: closed
author: copperfield42
labels:
  - bug
  - question
assignees: []
created_at: 2023-03-17T02:44:06Z
updated_at: 2023-03-17T21:57:47Z
url: https://github.com/astral-sh/ruff/issues/3568
synced_at: 2026-01-10T11:09:46Z
```

# ruff check file.py output is unreadable

---

_Issue opened by @copperfield42 on 2023-03-17 02:44_

Hello, I just hear of this project so I installed to test it, following the usage indication on some file I got this

![image](https://user-images.githubusercontent.com/4504719/225796639-1e76bda0-9a30-49ec-bfe3-3754c7f495b9.png)


I installed it like `pip install ruff`
ruff version: 0.0.256
Python version 3.11.2
OS: Windows 10


---

_Comment by @charliermarsh on 2023-03-17 02:45_

It looks like the color-support detection isn't working. Can you try setting `NO_COLOR=1`?

(Reference in docs: https://beta.ruff.rs/docs/faq/#how-can-i-disable-ruffs-color-output)

---

_Comment by @charliermarsh on 2023-03-17 02:45_

(Not sure _why_ it's not working, but we delegate to another library for that: https://crates.io/crates/colored)

---

_Label `question` added by @charliermarsh on 2023-03-17 02:45_

---

_Comment by @copperfield42 on 2023-03-17 02:52_

creating and setting the NO_COLOR enviroment variable like that I got

![image](https://user-images.githubusercontent.com/4504719/225800398-49f72aa1-f6e5-4ef8-991c-b068bbd02530.png)

now is readable


---

_Comment by @copperfield42 on 2023-03-17 02:56_

I guess I have to install that colored library too...
 

---

_Comment by @copperfield42 on 2023-03-17 03:03_

mmm apparently I have to download a 625mb installer to get the docker thing in order to install the colored library!? what? that is a little too much

---

_Comment by @charliermarsh on 2023-03-17 03:33_

No no sorry! You don't need to install that library at all. It's a library that Rust uses under the hood to output the colored escape characters, which many terminals recognize. It's _supposed_ to detect whether your terminal supports those characters, and only output them if it does. In your case, it looks like your terminal _doesn't_ support those characters, but the library is emitting them anyway. (E.g., on my machine, I didn't install that library outside of Ruff, but I do get colored output.)

In short: you don't need to install anything, but for whatever reason, it seems you either need to disable color via the environment variable (so that the library doesn't even _try_ to output color), or try in a different terminal.

---

_Comment by @copperfield42 on 2023-03-17 03:56_

that is weird, it display color correctly for other thing like those of pip, and with the [colorama](https://pypi.org/project/colorama/) library (that is needed for the tqdm to display correctly) also work fine, after calling the `just_fix_windows_console`

![image](https://user-images.githubusercontent.com/4504719/225807340-08e72729-d741-4b65-9a96-a913ec8740bc.png)

removing the environment and trying in the power shell I got the same

![image](https://user-images.githubusercontent.com/4504719/225807798-22757b31-4c73-4a39-b0af-6d250f3f1b09.png)


here is a test script with the colorama

```
from colorama import Fore, Back, Style, just_fix_windows_console

just_fix_windows_console()

print(Fore.RED + 'some red text')
print(Back.GREEN + 'and with a green background')
print(Style.DIM + 'and in dim text')
print(Style.RESET_ALL)
print('back to normal now')
```
commenting the just_fix_windows_console it display

![image](https://user-images.githubusercontent.com/4504719/225808520-5ff1190e-36b2-4aa0-ae92-999f091e7305.png)

with just_fix_windows_console 

![image](https://user-images.githubusercontent.com/4504719/225808614-8b8b9609-ff9c-44e8-87a9-838766103eec.png)

maybe there is something similar with that colored library too?

---

_Comment by @charliermarsh on 2023-03-17 03:58_

Hmm, I'm not 100% sure -- it says `Works on Linux, MacOS, and Windows (Powershell)` so it's at least _supposed_ to work in PowerShell.

---

_Comment by @charliermarsh on 2023-03-17 03:59_

There's some commentary in https://github.com/mackwic/colored/issues/59 where it seems like it doesn't work in some cases on Windows, but that it's not clear when.

---

_Comment by @charliermarsh on 2023-03-17 04:00_

Oh, I'm seeing this: https://docs.rs/colored/2.0.0/x86_64-pc-windows-msvc/colored/control/fn.set_virtual_terminal.html

---

_Comment by @charliermarsh on 2023-03-17 04:01_

Maybe I should be setting that on Windows?

---

_Label `bug` added by @charliermarsh on 2023-03-17 04:01_

---

_Comment by @copperfield42 on 2023-03-17 04:06_

well, lets try and see.
[constituent](https://github.com/mackwic/colored/issues/59#issuecomment-954367625) there also said something about that

---

_Comment by @charliermarsh on 2023-03-17 15:33_

Are you interested in building Ruff locally to test that?

---

_Comment by @copperfield42 on 2023-03-17 15:40_

sure, what I need to do?

---

_Comment by @charliermarsh on 2023-03-17 18:24_

Could you clone this branch (https://github.com/charliermarsh/ruff/pull/3583), then run:

```
cargo run -p ruff_cli -- /path/to/your/file.py --no-cache
```

---

_Comment by @charliermarsh on 2023-03-17 18:25_

You'd need to have Rust installed, so if that's too big of a lift, let me know. Maybe someone else can test.

---

_Comment by @copperfield42 on 2023-03-17 18:56_

it is big lift, Rust is very heavy, well the Visual Studio part is, that 950mb in just the c++ compiler or whatever holy molly and if it need the win 11 sdk is 1.5gb DX

---

_Comment by @copperfield42 on 2023-03-17 18:58_

if that branch can be made to be pip installed or similar without needing to install rust I can test it

---

_Comment by @charliermarsh on 2023-03-17 19:00_

Ok thanks, let me create a wheel.

---

_Comment by @charliermarsh on 2023-03-17 19:31_

I created wheels and binaries from that branch here: https://github.com/charliermarsh/ruff/actions/runs/4450737152

You can download them at the bottom, and find the appropriate one for your platform. LMK if that works...

---

_Comment by @copperfield42 on 2023-03-17 20:02_

![image](https://user-images.githubusercontent.com/4504719/226019978-c818f6a2-b95e-477c-b331-6419a5b17d4e.png)

it appear to work correctly now :)

---

_Comment by @charliermarsh on 2023-03-17 20:18_

Woohoo! I'll clean up that PR.

---

_Comment by @navneethc on 2023-03-17 20:45_

Can you use Windows Terminal?

---

_Comment by @copperfield42 on 2023-03-17 20:50_

the power shell? sure
![image](https://user-images.githubusercontent.com/4504719/226041584-01f92b6e-2a58-46ec-8dc2-32edf6bbf109.png)


---

_Comment by @navneethc on 2023-03-17 21:06_

> the power shell? sure ![image](https://user-images.githubusercontent.com/4504719/226041584-01f92b6e-2a58-46ec-8dc2-32edf6bbf109.png)

No, Windows Terminal -- it's the more modern alternative to the traditional terminal emulator. I'd recommend using that. It's available via the Windows Store: https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701

---

_Closed by @charliermarsh on 2023-03-17 21:08_

---

_Comment by @copperfield42 on 2023-03-17 21:13_

sure, give me a moment to install it

---

_Comment by @copperfield42 on 2023-03-17 21:16_

![image](https://user-images.githubusercontent.com/4504719/226052904-c58cb9c0-7552-452e-80a5-f7a0f1403d80.png)

in that one there is no issue with either version of ruff

---

_Comment by @navneethc on 2023-03-17 21:57_

> ![image](https://user-images.githubusercontent.com/4504719/226052904-c58cb9c0-7552-452e-80a5-f7a0f1403d80.png)
> 
> in that one there is no issue with either version of ruff

Yeah, that's why I recommended it. :)

---
