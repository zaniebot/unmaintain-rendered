```yaml
number: 12104
title: To be able to protect environments for scripts that remain on the system
type: issue
state: closed
author: andrewcrook
labels:
  - enhancement
assignees: []
created_at: 2025-03-10T19:30:38Z
updated_at: 2025-03-12T22:45:07Z
url: https://github.com/astral-sh/uv/issues/12104
synced_at: 2026-01-12T16:00:55Z
```

# To be able to protect environments for scripts that remain on the system

---

_@andrewcrook_

### Summary

### The Basic Idea 

To be able to protect environments for scripts that remain on the system.

Being able to run Python scripts and have them automatically install dependencies is extremely useful.  However,  when cleaning/pruning the uv cache, whilst it is able to preserve environments and dependences for projects, it will remove all environments for those scripts.

While the environment would be recreated upon next run, commonly used scripts would continually recreate environments after each clean or prune.

I propose a method to preserve script environments for scripts that remain on the system. 



### Example

### How would this work?

I suggest an argument `--env-protect` that will add data to the environment so it can be matched back to its script. So when cleaning or pruning the cache.  Any cached script environments that have an existing script on the system can be protected from removal. Cached  environments without this data will be processed as normal. This means when a script is removed from system the next clean/prune will remove its environment.

#### runing script 
- Add an optional  argument example `--env-protect` to hash bang
     `#!/usr/bin/env -S uv run --env-protect`
      when used the following happens ...
- Create environment and install the dependencies as normal
- if `--env-protect`  was used. Create new file or data added to existing file which has the absolute path of the script that the environment belongs to.
      `/.cache/uv/environments-v2/script-523ad544b121cf7a/ `
- run script

#### pruning/cleaning the cache 
  - scan cached environments for the aforementioned path of a script 
        -  if found, search for the script at path
                     -- if script is found, skip.
                      -- else the script is not found , continue as normal which will remove the environment.
        - if the scripts environment doesn’t have the scripts path continue as normal which will remove the environment.
 
 
- All functionality for projects would remain the same.


**There is one small issue which isn't too much of an issue to be honest.**

If a script is moved or renamed the environment would be removed, however, it would be recreated when next run.  

Out of interest, the only way I can think to solve both of these would be to store a script file's inode or equivalent. Example.  


```
 > ls -li
256166048 .rwxr-xr-x 206 andrew 10 Mar 15:44 test.py
 > find . -inum 256166048 -exec echo "{}" \;
./test.py
> mkdir test
> mv test.py test/
> find . -inum 256166048 -exec echo "{}" \;
./test/test.py
> mv test/test.py test/test2.py
> find . -inum 256166048 -exec echo "{}" \;
./test/test2.py
```

Not sure if this works for all file systems and I presume Windows NTFS uses an equivalent method and has tools.

---

_Label `enhancement` added by @andrewcrook on 2025-03-10 19:30_

---

_Comment by @zanieb on 2025-03-10 19:35_

I worry about adding more complexity here. It sounds like we just shouldn't remove environments for scripts on `uv cache prune`. I think `uv cache clean` should probably continue to do so.

Why do you need the environment to be retained in general? Are you frequently using `uv cache clean`?

---

_Comment by @zanieb on 2025-03-10 19:35_

cc @charliermarsh 

---

_Comment by @andrewcrook on 2025-03-10 19:50_

@zanieb 

> I worry about adding more complexity here. 

yeah I get that and you're right it does add an uncomfortable amount of complexity 

>  retained in general?

system scripts, automation and tools created 

> Are you frequently using uv cache clean?

I do use python a lot, and a lot for build scripts. I am always aware of package managers that can build up a huge stores overtime of what should be temporary. Most packet managers have a way to prune, clean or "garbage collect".

> It sounds like we just shouldn't remove environments for scripts on uv cache prune. I think uv cache clean should probably continue to do so.

May be don't remove them but add option to clean just scripts or an option to keep them. 
Would be useful to be able to list those environments so maybe can remove such environments by name as well?

Just running past some ideas

---

_Comment by @zanieb on 2025-03-10 19:55_

> > retained in general?
>
> system scripts, automation and tools created

Does this mean you're calling into the environment directly? Are you not using `uv run` to interface with the scripts? Is the overhead of recreating the environment large in your use-case?


---

_Comment by @andrewcrook on 2025-03-10 20:02_

For my tools I just have a directory of python scripts on path  of all different complexities.
I plan to use uv run but as a hashbang and use the new python pep for adding metadata.
I am thinking of moving everything over to this and let uv manage it because I don't really want to manage packages for pipx or venvs.

```
#!/usr/bin/env -S uv run

# /// script
# dependencies = [
#   "colorama",
# ]
# ///


from colorama import init, Fore, Style

init(autoreset=True)

print(f"d: {Fore.GREEN}This is a test {Style.RESET_ALL}”)
```

---

_Comment by @andrewcrook on 2025-03-11 19:12_

@zanieb  I wonder if you could please help me how is the script environment directory name generated?
obviously it uses the filename is that a hash, if so,  what of and what encoding?

```
~/.cache/uv/environments-v2/
drwxr-xr-x - andrew 10 Mar 17:14 test-523ad544b121cf7a
```

If I knew that, I could build a wrapper to do what I wanted.

---

_Comment by @zanieb on 2025-03-11 19:53_

The hash is generated as below

https://github.com/astral-sh/uv/blob/e843433b07db08bdabd64b4cfe822e57b3a31154/crates/uv/src/commands/project/mod.rs#L580

https://github.com/astral-sh/uv/blob/fb35875f24d923280aced423ecbe6a693d2d87ee/crates/uv-cache-key/src/digest.rs#L7-L19

What would the wrapper look like?

---

_Comment by @andrewcrook on 2025-03-12 13:21_

@zanieb 

> What would the wrapper look like?

Not sure yet probably a script that will be called from a function in a shell's rc script. A script that will pass most things straight  through to uv but allow me to override or add a few arguments calling some sort of script that interacts with UV.
Most likely Python to start with, if I can recreate the code you kindly gave me into Python unfortunately I am not familiar with Rust. 

---

_Comment by @zanieb on 2025-03-12 19:38_

I still don't quite understand why you need to determine the script directory path?

You can use `uv sync --script <path> --dry-run` though.

---

_Comment by @charliermarsh on 2025-03-12 19:40_

Have you considered using `--active` to control the path yourself?

---

_Comment by @andrewcrook on 2025-03-12 22:35_

@zanieb  @charliermarsh 

I need to look into all the UV arguments I am still new to using it. But thank you both I think I have enough to start looking into producing something over the weekend 

> I still don't quite understand why you need to determine the script directory path?

I dont think I do now 

> Have you considered using --active to control the path yourself?

This looks like what I want

Thanks both

---

_Closed by @andrewcrook on 2025-03-12 22:44_

---

_Comment by @charliermarsh on 2025-03-12 22:45_

Thanks for following up!

---
