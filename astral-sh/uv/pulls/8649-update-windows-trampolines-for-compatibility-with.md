```yaml
number: 8649
title: Update Windows trampolines for compatibility with Python 3.7 and earlier
type: pull_request
state: open
author: nahco314
labels:
  - compatibility
assignees: []
base: main
head: main
created_at: 2024-10-28T22:13:22Z
updated_at: 2025-07-02T05:31:47Z
url: https://github.com/astral-sh/uv/pull/8649
synced_at: 2026-01-10T06:53:01Z
```

# Update Windows trampolines for compatibility with Python 3.7 and earlier

---

_Pull request opened by @nahco314 on 2024-10-28 22:13_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

uv-trampoline uses Python's zipimport feature in a slightly hacky way to provide exe files, but the current method causes errors in Python 3.7 and earlier.

Specifically, uv-trampoline stores the Python executable path and other information as archive comments in the zip file. However, zipimport before Python 3.7 does not support archive comments (https://docs.python.org/3/library/zipimport.html#:~:text=Changed%20in%20version%203.8%3A%20Previously%2C%20ZIP%20archives%20with%20an%20archive%20comment%20were%20not%20supported ).

When I actually run the installed exe with Python 3.7 on Windows, I get the following puzzling error.
```
$ pycowsay
  File "C:\Users\nahco\.local\bin\pycowsay.exe", line 1
SyntaxError: Non-UTF-8 code starting with '\x83' in file C:\Users\nahco\.local\bin\pycowsay.exe on line 2, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details
```

This PR will support Python 3.7 and earlier by moving the path to Python, etc. (as well as the exe file content) before the zip portion.

### Is this necessary?
Of course, Python 3.7 is EOL.
But it is still used in practice and as long as it is installable with uv, it is a good thing to support it. The change to support it is simple and will not change the cost of maintenance much.

Also, if this change is unacceptable, I think we should display an error indicating that the exe is not supported in Python 3.7 or earlier. In that case I am still willing to create a PR.

## Test Plan

<!-- How was it tested? -->

Simply install and run some tool using Python 3.7.
I added this for now because I don't really understand the uv code based testing method, but I will fix it if there are any problems.


---

_Comment by @zanieb on 2024-10-28 22:18_

Thanks for contributing!

Unfortunately, I also worked on the trampoline today and this will conflict with #8637 – I presume that can be reconciled though.

I'm a +1 on this. We'll want to hear from @konstin as well.

cc @samypr100 if you're interested, I know you've done a fair bit of work on the trampolines. 

---

_Label `compatibility` added by @zanieb on 2024-10-28 22:19_

---

_Review requested from @konstin by @zanieb on 2024-10-28 22:19_

---

_Comment by @zanieb on 2024-10-28 22:20_

I am a bit confused since I thought the trampolines were written when 3.7 was not EOL — did we break support at some point? 

---

_Comment by @nahco314 on 2024-10-28 22:47_

I'm not sure, but at the time the trampoline in posy was created, 3.7 must have been somewhat old, so it may simply have gone unnoticed.

---

_Review comment by @konstin on `crates/uv-trampoline/src/bounce.rs`:125 on 2024-10-29 08:25_

Can we use https://doc.rust-lang.org/std/io/trait.Read.html#method.read_exact instead?


---

_Review comment by @konstin on `crates/uv-trampoline/src/bounce.rs`:139 on 2024-10-29 08:26_

This can be `unreachable!()`

---

_@konstin reviewed on 2024-10-29 08:28_

I'm -1 on supporting Python 3.7. Python 3.7 was already EOL when uv was released, and users should upgrade asap rather than patching the support into tools. Otherwise the code looks good.

---

_Comment by @T-256 on 2024-10-29 11:23_

Previously raised issue: https://github.com/astral-sh/uv/issues/2445.
For some works I'm stuck at Python 3.7. On Windows I have this problem that I cannot run apps/scripts directly after activating venv, or neither via `uv run`.
There is limited workaround: `uv run python -m <MODULE>` (only for those has same script name exposed as module name).

I can verify uv fully works on Python 3.7 on Windows and the only issue remained is that trampoline issue.

---

_Comment by @samypr100 on 2024-10-29 12:11_

-1 for 3.7 support.
I'm not sure if the added complexity is worth it given now that 3.8 is also effectively EOL, so we'd be supporting two EOL versions. In addition, it adds additional overhead to the developer when testing as they'd have to test 3.7 doesn't break the trampolines to @notatallshaw 's point in https://github.com/astral-sh/uv/issues/7418#issuecomment-2359628408

---

_Comment by @T-256 on 2024-10-29 12:38_

That's correct both 3.7 and 3.8 EOL, but I think uv is still able to support them as in:
https://github.com/astral-sh/uv/blob/9fa4fea8f24c6115366244fbc920fb091a1ad53a/crates/uv-python/src/discovery.rs#L1767-L1770

- Also, see https://github.com/astral-sh/uv/issues/8255



---

_Comment by @zanieb on 2024-10-29 13:41_

One problem here is that I need to be able to sniff executables to tell if they're trampolines. I feel like that's trivial with the magic number at the end but kind of annoying here? If we're inverting the format I guess I'd expect

```
|       `launcher.exe`        |
| :-------------------------: |
| `<b'U', b'V', b'U', b'V'>`  |
| `<len(path to python.exe)>` |
|   `<path to python.exe>`    |
|  `<zipped python script>`   |
```

right?

---

_Comment by @nahco314 on 2024-10-29 23:31_

It would be difficult to put the magic number of the UV in a trivial position, since the magic number of the exe must be placed at the beginning of the file and the EOCD at the end.
The inverted version is also difficult because the length of the exe file is non-trivial. (The length of the exe file is known in advance, but this may not be very useful since it can change from version to version.)

I think it is possible to insert the magic number in the last 2 bytes of the EOCD (where the length of the comment is stored). Whether this will work is implementation-dependent, but a normal zip implementation should work fine.

```
$ ./.venv/Scripts/black.exe --version
black, 23.3.0 (compiled: yes)
Python (CPython) 3.7.9

$ od -c ./.venv/Scripts/black.exe | tail -n 3
0125700 005 006  \0  \0  \0  \0 001  \0 001  \0   9  \0  \0  \0   9 001
0125720  \0  \0   U   V
0125724
```

---

_Comment by @zanieb on 2024-10-29 23:45_

Yeah sorry, I forgot that there's the entire trampoline executable itself at the front.

>  (The length of the exe file is known in advance, but this may not be very useful since it can change from version to version.)

We could hard-code this, I guess. Or pad to a specific size.

I basically need to be able to inspect the trampoline, as in https://github.com/astral-sh/uv/blob/9cb7bcfa7aa570f6a0c79d1698d7c849dfa716be/crates/uv-trampoline-builder/src/lib.rs#L39-L123

Notably with #8637 there isn't any zip payload at all in this variant, which perhaps simplifies things — we just won't be able to inspect the script variants.

---

_Comment by @nahco314 on 2024-10-30 00:17_

Anyway, I will read changes in #8637 .

---

_Comment by @samypr100 on 2024-10-30 01:34_

Generally speaking I'd assume you'd always want the magic at the `beginning` or `end` (not in-between) to avoid issues with the magic casually popping up in-between twice due to other reasons (e.g. particular exe ends up having the magic repeated mid-way) which can happen often depending on the magic.

---

_Comment by @nahco314 on 2024-10-30 02:41_

If we parse the EOCD and read the magic number in the middle of the file, the probability of false positives is as low as with the current method (or more).
Perhaps the problem is that the implementation would be somewhat more complicated. If we should to avoid that, padding the exe file sounds like a good idea.

---

_Comment by @nahco314 on 2024-10-30 02:49_

One idea:  In the case of UVSC, embedding "UV" in the comment length section of the EOCD and using the EOCD's magic number (PK\005\006) for detection allows for a robust and reliable method that only requires static reading.
For UVPY, simply placing "UVPY" at the end is sufficient.

---

_Comment by @nahco314 on 2025-01-14 14:02_

I neglected this for a while, but I've now updated it to match the latest main!

- For now, I've kept the magic number placed before the zip content (as before). This makes the parsing process a bit more complex, but the probability of false detection shouldn’t be an issue. Essentially, it's the same in that it relies on the probability that a specific 4 bytes appears at a specific location in the file.  
- As for tests, there are unit tests in `uv-trampoline-builder`, so I believe those are sufficient.

In the end, whether or not we need 3.7 support is up to the maintainers, but I’ve made it so that we can merge this anyway.

---

_Review comment by @T-256 on `crates/uv-trampoline-builder/src/lib.rs`:170 on 2025-01-14 16:35_

function return type could be `Result<Self, Error>`.

---

_Review comment by @T-256 on `crates/uv-trampoline/README.md`:103 on 2025-01-14 16:36_

this section should be updated to `UVSC` and `UVPY`

---

_@T-256 approved on 2025-01-14 16:36_

Nice, two comments that also can add them as follow-up.

---

_@nahco314 reviewed on 2025-01-14 16:51_

---

_Review comment by @nahco314 on `crates/uv-trampoline-builder/src/lib.rs`:170 on 2025-01-14 16:51_

Thank you for your review and suggestion!
However, since it is LauncherKind::try_from_file that determines whether the file is actually a launcher, I believe returning Ok(None) makes sense when the file is not recognized as one.

---

_@T-256 reviewed on 2025-01-14 17:13_

---

_Review comment by @T-256 on `crates/uv-trampoline-builder/src/lib.rs`:170 on 2025-01-14 17:13_

> I believe returning Ok(None) makes sense when the file is not recognized as one.

I think a meaningful `Error` variant also does the job.

```diff
-        let Ok(_) = file.seek(io::SeekFrom::End(
-            -((skip_bytes + MAGIC_NUMBER_SIZE) as i64),
-        )) else {
-            return Ok(None);
-        };
+        file.seek(io::SeekFrom::End(
+            -((skip_bytes + MAGIC_NUMBER_SIZE) as i64),
+        )).map_err(Error::InvalidLauncherSeek)?;  # <-- I think this is more proper return than `Ok(None)`

...

- Ok(Self::try_from_bytes(buffer))
+ Self::try_from_bytes(buffer).ok_or(Error::InvalidMagic)
```


---

_@nahco314 reviewed on 2025-01-14 23:40_

---

_Review comment by @nahco314 on `crates/uv-trampoline-builder/src/lib.rs`:170 on 2025-01-14 23:40_

Are you suggesting that, rather than just changing LauncherKind::try_from_path, we should also implement Launcher::try_from_path so that it does not return an Option in the first place?

---

_@nahco314 reviewed on 2025-02-19 14:33_

---

_Review comment by @nahco314 on `crates/uv-trampoline-builder/src/lib.rs`:170 on 2025-02-19 14:33_

Error handling changes a little, but the operation is fine, so I switched to this method.

---

_Review comment by @T-256 on `crates/uv-trampoline/README.md`:110 on 2025-02-19 22:01_

I think it's better to have them separated to correctly show them in table:

```md
### How do you use it as Python mirror?

Basically, this invokes the provided `python.exe` executable.

The intended use is:

- First, place our prebuilt `.exe` content at the top of the file.
- After the exe file content, write the path to the Python executable that the script uses to run
  the Python script as UTF-8 encoded string, followed by the path's length as a 32-bit little-endian
  integer.
- Write the magic number 'UVPY' in bytes.

|       `launcher.exe`        |
| :-------------------------: |
|   `<path to python.exe>`    |
| `<len(path to python.exe)>` |
| `<b'U', b'V', b'P', b'Y'>`  |

### How do you use it as Script runner?

Basically, this looks up `python.exe` (for console programs) and invokes
`python.exe path\to\the\<the .exe>`.

The intended use is:

- First, place our prebuilt `.exe` content at the top of the file.
- After the exe file content, write the path to the Python executable that the script uses to run
  the Python script as UTF-8 encoded string, followed by the path's length as a 32-bit little-endian
  integer.
- Write the magic number 'UVSC' in bytes.
- Finally, rename your Python script as `__main__.py`, compress it into a `.zip` file, and append
  this `.zip` file to the end of one of our prebuilt `.exe` files.

|       `launcher.exe`        |
| :-------------------------: |
|   `<path to python.exe>`    |
| `<len(path to python.exe)>` |
| `<b'U', b'V', b'S', b'C'>`  |
|  `<zipped python script>`   |
```

---

_@T-256 approved on 2025-02-19 22:14_

Thanks, it looks great!
For reviewers' interests: uv somehow exposed its support of Python 3.7 via [`uv python`](https://github.com/astral-sh/uv/blob/main/crates/uv-python/download-metadata.json) where Python 3.7 is lowest version of available distributions.

---

_Review requested from @konstin by @nahco314 on 2025-02-22 09:50_

---

_Comment by @charliermarsh on 2025-03-03 04:00_

It looks like this came up again in https://github.com/astral-sh/uv/issues/11847.

---

_Comment by @T-256 on 2025-03-06 12:21_

Any thoughts on what is the blocker here?
We can always revert this (non-breaking) change when final decision made into #8255.

---

_Closed by @nahco314 on 2025-03-11 05:31_

---

_Reopened by @nahco314 on 2025-03-11 05:32_

---

_Comment by @nahco314 on 2025-03-11 05:34_

Sorry, I closed it by mistake 🙇‍♂️

---

_Comment by @nahco314 on 2025-03-11 06:35_

By the way, I echo T-256's point that merging this PR first likely won’t cause much harm (as it can be easily rolled back if needed). But I’d love to hear your thoughts on whether that approach would be acceptable.

Anyway, if it looks like this might take a while longer to merge, perhaps we should consider adding a temporary patch that shows a warning or error on Windows with Python 3.7.

---

_Comment by @elonzh on 2025-05-21 05:56_

@zanieb @charliermarsh 
We are currently attempting to manage our Python environments and dependencies using uv. Unfortunately, our production application uses Python 3.7. 

Upgrading Python is contingent on us first using uv to manage and lock dependencies, ensuring that the Python upgrade will not introduce breaking changes. Judging from community feedback, quite a few users have encountered this issue while using uv, and I'm wondering if their scenarios are similar to ours. 

Although Python 3.7 has indeed reached its end-of-life, we still very much hope this PR can be merged, as it would be immensely helpful for migrating to uv and upgrading our Python version.

---

_Comment by @T-256 on 2025-06-03 00:16_

FWIW, I just opened a project which is un-upgradable (it works only with Python37 `ast` and `marshal` validating). it'd nice to run its executables via `uv run`.

@nahco314 Can you resolve conflicts of this PR, then I think I can deploy your branch and manually replace generated artifacts while using latest features of uv :)

---

_Comment by @nahco314 on 2025-06-04 12:14_

conflict just resolved! @T-256 

---

_Comment by @nahco314 on 2025-06-04 12:25_

By the way, according to #13022, has uv decided on dropping support for Python 3.7?
If that's the case, this PR would no longer be necessary and should be closed.

---

_Comment by @zanieb on 2025-06-04 13:31_

No, https://github.com/astral-sh/uv/pull/13022 does not mean we won't attempt to retain compatibility with 3.7.

---

_Comment by @zanieb on 2025-06-04 13:33_

> Any thoughts on what is the blocker here?

The blocker is that we are hesitant to change an already complicated part of uv to support 3.7 (a version outside of both our and upstream support ranges), especially in a way that adds more complexity.

---

_Comment by @T-256 on 2025-06-05 03:00_

> we are hesitant to change an already complicated part of uv

IIUC, implementation complexity of trampoline was described in `crates/uv-trampoline/README.md` for readers/researchers. IMO, in this PR, [that document](https://github.com/astral-sh/uv/pull/8649/files#diff-1f39d28be1834a2813506fd9b872056a68cfd2569d281148dcd4f076c5b87049) has improved.

> (a version outside of both our and upstream support ranges)

I agree this version of Python is out of support now, but I see this PR has been labeled as `compatibility`. According to CI, Python3.9+ actively tested but docs didn't specify _compatible_ ranges of Python version, I assume it must be `>=3.5` (?). So, this PR tries to fix a compatibility issue.

Let's at least have one published version of uv with this change, so we can reference it with when related issues comes up for Python3.7 trampolines and check if that issue still reproducible with that version. We can revert it back when there is problem without no harm.

FWIW, currently there are 4 tiers of Python versions in uv:
1. `uv python` supported and actively tested: `>=3.9`
2. `uv python` supported only: `3.8`
3. compatibility supportyed only: `3.7`
4. unsupported: `<3.7` (see also: https://github.com/astral-sh/uv/issues/7418)

If decided to not merge this PR, I'd recommend blocking Python 3.7 in here:
https://github.com/astral-sh/uv/blob/90a4416ab862de640aee78c19017873a9b40fd68/crates/uv-python/python/get_interpreter_info.py#L485-L488

---

_Comment by @zanieb on 2025-06-05 13:03_

> Let's at least have one published version of uv with this change, so we can reference it with when related issues comes up for Python3.7 trampolines and check if that issue still reproducible with that version. We can revert it back when there is problem without no harm.

That's.. not how it works? It causes harm for us to release regressions, even if we revert it.

---

_Comment by @T-256 on 2025-06-05 15:31_

> That's.. not how it works? It causes harm for us to release regressions, even if we revert it.

I suggest keeping it and don't revert, but can you expand that harm you mean with this change?


---

_Review request for @konstin removed by @konstin on 2025-06-18 15:53_

---

_Comment by @elonzh on 2025-07-02 05:31_

@zanieb Can this PR be merged?

---
