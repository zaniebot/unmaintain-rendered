```yaml
number: 579
title: Do not use mmap by default.
type: pull_request
state: closed
author: samhocevar
labels: []
assignees: []
base: master
head: fix-disable-mmap-by-default
created_at: 2017-08-22T14:43:03Z
updated_at: 2017-08-23T15:18:02Z
url: https://github.com/BurntSushi/ripgrep/pull/579
synced_at: 2026-01-12T18:23:13Z
```

# Do not use mmap by default.

---

_@samhocevar_

Reading memory-mapped files is inherently insecure because no safe
mechanism exists to prevent out-of-bounds reads if the file gets
truncated. The POSIX standard says “An implementation may deliver
SIGBUS signals when a reference would cause an error in the mapped
object, such as out-of-space condition.” so it seems safer to disable
mmap by default.

The bug is simply triggered using this command:

    # dd of=zob count=0 seek=16G; (sleep 0.1; truncate -s1 zob)&; rg -a lol zob
    [2]    15028 bus error  rg -a lol zob

---

_Comment by @BurntSushi on 2017-08-22 18:38_

Note that the definition of "safety" you're using doesn't seem to align with Rust's definition of safety. That is, if SIGBUS is guaranteed for out-of-bounds indexing, then there is no memory unsafety here since SIGBUS will terminate the process.

Disabling memory maps by default because you can trigger a SIGBUS with a pathological case doesn't seem like a strong enough motivation to me. Is there some other motivation you'd like to put forward? What are the end user facing ramifications of the unsafety that you speak of?

---

_Comment by @samhocevar on 2017-08-23 11:24_

My motivation is that *another user on the system* can make my process crash with an uncaught `SIGBUS`, and I like my applications to not crash when merely reading files.

---

_Comment by @BurntSushi on 2017-08-23 11:30_

> My motivation is that another user on the system can make my process crash with an uncaught SIGBUS, and I like my applications to not crash when merely reading files.

OK, then I'd suggest adding an alias that disables memory maps, for example, `alias rg="rg --no-mmap"`. That works today.

Personally, I'm inclined to think your use case is pretty niche (how often has ripgrep crashed on you because of this?), and I'm not convinced it's worth it to disable faster search by default because of it.

---

_Comment by @samhocevar on 2017-08-23 11:45_

You do not seem to agree that this is a security bug. I do not know how I can convince you that it is; if the Rust community has a security team, I suggest asking them for their opinion.

---

_Comment by @hadronized on 2017-08-23 11:48_

Isn’t there a way to still have mmap enabled and catch the signal upfront? If a `SIGBUS` is detected, abort everything as there’s no stable answer.

You’d have a return code – and security teams will be happy – and performance will remain exactly the same.

---

_Comment by @BurntSushi on 2017-08-23 11:59_

> You do not seem to agree that this is a security bug. I do not know how I can convince you that it is

You can convince me by focusing on the end user ramifications here. From what I've seen from you so far, the ramification is that another user on the same system as you can cause your process to abort. Is there some vulnerability that can result from this?

---

_Comment by @BurntSushi on 2017-08-23 12:02_

> Isn’t there a way to still have mmap enabled and catch the signal upfront? If a SIGBUS is detected, abort everything as there’s no stable answer.

Why does the signal need to be caught? If the process receives SIGBUS, doesn't the process subsequently terminate?

---

_Comment by @samhocevar on 2017-08-23 12:02_

> From what I've seen from you so far, the ramification is that another user on the same system as you can cause your process to abort. Is there some vulnerability that can result from this?

That is the definition of a local denial of service vulnerability.

---

_Comment by @BurntSushi on 2017-08-23 12:06_

> That is the definition of a local denial of service vulnerability.

Could you please help me understand the actual issue here instead of quoting definitions? I'm trying to understand your use case, and you aren't making it easy.

For example, am I correct in assuming that the only way this can happen is if the other end user has write access to a file you're trying to search? If that's true, then I don't really understand why a DoS of this nature is important to you. Could you elaborate more on when this happens?

Could you also say why passing the `--no-mmap` flag is insufficient? Would you be happier if the documentation said that ripgrep's defaults were optimized for a single user machine?

---

_Comment by @flure on 2017-08-23 12:14_

I think it's more logical to have an option `--fast-but-may-crash` rather than an option `--safe-but-slower`. Whatever the security issues are.

---

_Comment by @hadronized on 2017-08-23 12:18_

Is `mmap` the right default, anyway? Is it really faster than loading into via copy streaming?

---

_Comment by @samhocevar on 2017-08-23 12:18_

> Would you be happier if the documentation said that ripgrep's defaults were optimized for a single user machine?

That would not be accurate, because the bug exists on a single user machine, too. Maybe I would be “happier” if the documentation said that ripgrep can abort unexpectedly while reading files unless `--no-mmap` is specified, but I would be even happier if the default behaviour was to never crash.

Also, here is what GNU grep did in 2010:

        grep: remove --mmap
        mmap is a bad idea for sequentially accessed file because it will cause
        a page fault for every read page.  Just consider it a failed experiment,
        and ignore --mmap while accepting it for backwards compatibility.


---

_Comment by @samhocevar on 2017-08-23 12:23_

> For example, am I correct in assuming that the only way this can happen is if the other end user has write access to a file you're trying to search? If that's true, then I don't really understand why a DoS of this nature is important to you. Could you elaborate more on when this happens?

Your assumption is correct. But it means that even as root, one cannot reliably search through the files of all users with ripgrep.

The problem with security issues is not when they happen, but if they can happen, and in this case they can, so they need to be fixed (or at least documented).

---

_Comment by @BurntSushi on 2017-08-23 12:31_

> I think it's more logical to have an option --fast-but-may-crash rather than an option --safe-but-slower. Whatever the security issues are.

@flure It seems to me that this PR is trying to change the default. The `--mmap` and `--no-mmap` flags already exist. By default, ripgrep tries to choose between memory maps and buffered reading, depending on which it thinks will be faster (a heuristic). If we never permit memory maps to be used by default, then we leave easy performance gains on the table by default.

> Is mmap the right default, anyway? Is it really faster than loading into via copy streaming?

@phaazon No, it's not the right default *all the time*. It's the right default *some of the time*. Please look at the heuristic this PR is removing, and it's also discussed in [ripgrep's benchmarks](http://blog.burntsushi.net/ripgrep/#single-file-benchmarks).

> That would not be accurate, because the bug exists on a single user machine, too. Maybe I would be “happier” if the documentation said that ripgrep can abort unexpectedly while reading files unless --no-mmap is specified, but I would be even happier if the default behaviour was to never crash.

I would be OK with adding that verbiage to the docs (`rg -h`, `rg --help`, and `man rg`), so long as it includes *why* ripgrep would abort, e.g., "ripgrep can abort unexpectedly by default if it searches a file that is simultaneously truncated. This behavior can be disabled by passing the `--no-mmap` flag."

I don't see how changing the default behavior here is wise. If this were a bug that people were tripping over constantly, then I would be more sympathetic to your concern. But as far as I can tell, this bug is rare enough that leaving performance on the table isn't worth it.

> Also, here is what GNU grep did in 2010:

Right, I'm aware of GNU grep's history here. I purposefully re-litigated their experiment because I didn't believe their conclusion, and thus far, the data says their conclusion is wrong now (it may have been right in 2010, I don't know).

> Your assumption is correct. But it means that even as root, one cannot reliably search through the files of all users with ripgrep.

Understood. I'm personally OK with the trade off because if you want to do that, you can pass the `--no-mmap` flag.

> The problem with security issues is not when they happen, but if they can happen, and in this case they can, so they need to be fixed (or at least documented).

I understand that. But not all security issues are created equal, and security doesn't trump everything else all the time IMO.

-----

To summarize, it seems like a compromise here is to add a warning to the docs, but to leave the current defaults in tact.

---

_Comment by @samhocevar on 2017-08-23 12:47_

> I don't see how changing the default behavior here is wise.

I know, hence my suggestion to ask a security team. For instance, I do not see any sane Linux distro knowingly shipping ripgrep with this default.

---

_Comment by @BurntSushi on 2017-08-23 13:00_

> I know, hence my suggestion to ask a security team.

Oh? Is there something else to this issue that I'm missing? Happy to learn. :-)

> For instance, I do not see any sane Linux distro knowingly shipping ripgrep with this default.

I'm fine with only insane Linux distros shipping ripgrep. In fact, I use such a Linux distro!

But if sane Linux distros want a piece of the action, then I'd be honored, and they can come to me with their concerns, and I'm confident we can figure something out. :-)

---

_Comment by @samhocevar on 2017-08-23 15:12_

> I'm fine with only insane Linux distros shipping ripgrep.

Just for the record, I said no such thing.

---

_Comment by @BurntSushi on 2017-08-23 15:18_

I think PR has exhausted constructive feedback.

If someone would like to propose a fix, please open a new issue and focus on the user experience. Thanks. In the mean time, when I get a chance, I'll improve the documentation to cover the trade offs here.

---

_Closed by @BurntSushi on 2017-08-23 15:18_

---
