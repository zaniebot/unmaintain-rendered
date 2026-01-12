```yaml
number: 9531
title: "Unable to install a python project depending on `httpx` on a corporate Jenkins Windows Runner with AV"
type: issue
state: closed
author: Coruscant11
labels:
  - external
assignees: []
created_at: 2024-11-29T20:26:00Z
updated_at: 2025-04-02T18:29:42Z
url: https://github.com/astral-sh/uv/issues/9531
synced_at: 2026-01-12T15:59:52Z
```

# Unable to install a python project depending on `httpx` on a corporate Jenkins Windows Runner with AV

---

_@Coruscant11_

Hello!

So for the context of #9524, I was looking at an issue I am currently facing in my company.

Here is the error:
```shell
TRACE Extracting file name=PackageName("httpx")
TRACE Extracted 30 files name=PackageName("httpx")
TRACE Writing entrypoints name=PackageName("httpx")
DEBUG Failed to sync environment; removing `my_project`
DEBUG Deleting environment for tool `my_project` at path\to\my\my_project
DEBUG Released lock at `C:\workspace\workspace\MY_PR_WORKSPACE\folder\my_project-1cbe0bb525fecad3fccc339f295104c77d42e51c\tools\.lock`
error: Failed to install: httpx-0.27.2-py3-none-any.whl (httpx==0.27.2)
  Caused by: Failed to persist temporary file to folder\my_project-1cbe0bb525fecad3fccc339f295104c77d42e51c\tools\my_project\Lib\site-packages\..\..\Scripts\httpx.exe: Access is denied. (os error 5)
```

I also reproduce by simply trying`uv tool install httpx`

I am currently suspecting the AV to lock the `.exe` script of `httpx`.
I opened an issue on the `tempfile` crate with more details on this, and a fix idea: https://github.com/Stebalien/tempfile/issues/316
Could you please look at it and give me your opinion on it? (Most specifically with what `graceful-fs` tries to solve and how it does it: https://www.npmjs.com/package/graceful-fs) 

Thanks again!!

---

_Comment by @samypr100 on 2024-11-29 20:36_

Is there more details on the AV vendor / configuration? I'm curious if there's known workarounds. For example, would it help if you define the environment variables `TMP` or `TEMP` to a known persisted temporary location when running uv rather than relying on the default temporary directory location for windows?

We override `TMP` / `TEMP` for performance reasons in our [CI tests](https://github.com/astral-sh/uv/blob/main/.github/workflows/setup-dev-drive.ps1#L20-L21), maybe a similar approach might help your situation unless the file locking by the AV happens in any case where an executable is being moved.

---

_Label `upstream` added by @samypr100 on 2024-11-29 20:38_

---

_Comment by @Coruscant11 on 2024-11-29 20:42_

@samypr100, doesn't change anything.
I used custom cache and data dir, state, everything.
> the AV happens in any case where an executable is being moved.

Yes indeed, it happens every time 🙁 

I don't know if I can share it, but the AV is a very well known one. 

>  I'm curious if there's known workarounds
I really think that is it what `graceful-fs` is doing.. 
>On Windows, it retries renaming a file for up to one second if EACCESS or EPERM error occurs, likely because antivirus software has locked the directory.

I will try tomorrow a branch of tempfile trying this workaround, to validate if it fixes it or not !



---

_Comment by @Coruscant11 on 2024-11-29 20:45_

Something that I forgot to mention, is that the behavior is a little bit random, like there is 1/50 chance of having the installation work on the system having the issue.

(Most probably due to the AV lock behavior)

Tomorrow I will prototype a custom version of `uv` using a custom version of `tempfile` with a retry mechanism and validate if it can solve the issue or not. I will keep you updated!

---

_Comment by @Coruscant11 on 2024-11-30 14:35_

Really funny, just before submitting my pull-request, I found at that I re-implemented almost EXACTLY an already existing function ! 😆 
https://github.com/astral-sh/uv/blob/22fd9f7ff17adfbec6880c6d92c162e3acb6a41c/crates/uv-fs/src/lib.rs#L221

Like, almost exactly!

```rust
/// On Windows, A/V and/or EDR software can lock the file (most of the time executables), causing this
/// to fail with an EACCES or EPERM. Try again on failure, for up to the given timeout in seconds.
/// A timeout of at least 60 seconds, this long, is recommended because some Windows Anti-Virus, such as Parity
/// bit9 or Crowdstrike Falcon Sensor, may lock files for up to a minute
/// causing in our case dependencies scripts installation failures.
/// Took inspiration from graceful-fs, used by npm: <https://github.com/isaacs/node-graceful-fs/blob/234379906b7d2f4c9cfeb412d2516f42b0fb4953/polyfills.js#L87>
/// See more in their README: <https://www.npmjs.com/package/graceful-fs>
fn rename_with_retries_on_permission_denied<P, Q>(
    from: &P,
    to: &Q,
    timeout_in_seconds: u64,
) 
-> Result<(), Error> 
where P: AsRef<Path>, Q: AsRef<Path>
{
    // Start a timer to track how long we've been trying to install the script
    let start = Instant::now();
    let mut backoff = 0;

    while let Err(err) = fs::rename(from, to) {

        // If we've been trying for more than 60 seconds, abort and return an error
        if start.elapsed() > Duration::from_secs(60) {
            // Return formatted error message
            return Err(Error::Io(io::Error::new(
                io::ErrorKind::Other,
                format!(
                    "Failed to persist temporary file to {}: {}",
                    to.user_display(),
                    err
                ),
            )));
        }

        // In case of permission denied error, we retry until up to 60 seconds
        if err.kind() == io::ErrorKind::PermissionDenied {
            warn_user_once!(
                "Encountered a permission denied error while persisting temporary file to {}: retrying until up to {} seconds",
                to.user_display(),
                timeout_in_seconds,
            );

            std::thread::sleep(Duration::from_millis(backoff)); // Sleep before next retry
            backoff = backoff.saturating_add(10); // Wait 10ms longer each time
        } 

        // This is an unexpected error, let's abort
        else {
            return Err(Error::Io(io::Error::new(
                io::ErrorKind::Other,
                format!(
                    "Failed to persist temporary file to {}: {} seconds: {}",
                    to.user_display(),
                    timeout_in_seconds,
                    err
                ),
            )));
        }
    }

    Ok(())
}
```

I will remove all I did and just use what already exists then 😄 

It's fine ! It means it was a known issue, and the good solution

---

_Closed by @charliermarsh on 2024-12-01 22:57_

---

_Comment by @g0di on 2025-01-23 16:29_

I'm getting the original error while trying to install `pdm` on Windows using `uv` version `0.5.23`.

```bash
$ uv version
uv 0.5.23 (ba42467f1 2025-01-23)
```

```bash
$ uv tool install pdm
Resolved 36 packages in 367ms
error: Failed to install: httpx-0.28.1-py3-none-any.whl (httpx==0.28.1)
  Caused by: Failed to persist temporary file to C:\Users\benoit\AppData\Roaming\uv\tools\pdm\Lib\site-packages\..\..\Scripts\httpx.exe: failed to persist temporary file: The system cannot find the file specified. (os error 2)
```

However, using `uvx` instead works properly
```bash
$ uvx pdm --version
PDM, version 2.22.2
```

Shall we reopen?

---

_Comment by @charliermarsh on 2025-01-23 16:39_

You can reproduce this repeatedly?

---

_Comment by @g0di on 2025-01-23 16:48_

Yes

```bash
$ ./uv tool install pdm --reinstall
Resolved 36 packages in 3.70s
Prepared 36 packages in 214ms
error: Failed to install: httpx-0.28.1-py3-none-any.whl (httpx==0.28.1)
  Caused by: Failed to persist temporary file to C:\Users\benoit\AppData\Roaming\uv\tools\pdm\Lib\site-packages\..\..\Scripts\httpx.exe: failed to persist temporary file: The system cannot find the file specified. (os error 2)

$ ./uv tool install pdm
Resolved 36 packages in 2.89s
error: Failed to install: httpx-0.28.1-py3-none-any.whl (httpx==0.28.1)
  Caused by: Failed to persist temporary file to C:\Users\benoit\AppData\Roaming\uv\tools\pdm\Lib\site-packages\..\..\Scripts\httpx.exe: failed to persist temporary file: The system cannot find the file specified. (os error 2)

$ ./uv tool install pdm
Resolved 36 packages in 473ms
error: Failed to install: httpx-0.28.1-py3-none-any.whl (httpx==0.28.1)
  Caused by: Failed to persist temporary file to C:\Users\benoit\AppData\Roaming\uv\tools\pdm\Lib\site-packages\..\..\Scripts\httpx.exe: failed to persist temporary file: The system cannot find the file specified. (os error 2)

$ ./uv tool install pdm
Resolved 36 packages in 383ms
error: Failed to install: httpx-0.28.1-py3-none-any.whl (httpx==0.28.1)
  Caused by: Failed to persist temporary file to C:\Users\benoit\AppData\Roaming\uv\tools\pdm\Lib\site-packages\..\..\Scripts\httpx.exe: failed to persist temporary file: The system cannot find the file specified. (os error 2)
```

---

_Comment by @charliermarsh on 2025-01-23 17:07_

Do other versions of uv succeed?

---

_Comment by @Gankra on 2025-01-23 19:03_

Also do you have any particular antivirus running like the original issue mentions?

I can't reproduce this on my machine, but it's very plausible #10442 caused this, as it significantly refactors the change that fixed this issue. If so your usecase would be broken on uv [0.5.23](https://github.com/astral-sh/uv/releases/tag/0.5.23) through [0.5.17](https://github.com/astral-sh/uv/releases/tag/0.5.17).

Would be interesting to know if uv 0.5.16 works fine.

---

_Comment by @g0di on 2025-01-24 09:00_

> Do other versions of uv succeed?

It doesnt work as well on  `0.5.16` (same error).

> Also do you have any particular antivirus running like the original issue mentions?

I'm using a company laptop which has antivirus and other security tools scanning files and stuff in the background. This might be the cause of the issue. Unfortunately I can't turn those off.



---

_Comment by @g0di on 2025-01-24 10:08_

Funny enough, I've tried with a very older version of uv (0.4.0) which actually worked fine.

---

_Comment by @Gankra on 2025-01-24 14:16_

Interesting, thanks for all the useful info!

---

_Comment by @DarrenPadraigDV on 2025-04-02 03:04_

@charliermarsh I have a pretty similar issue I suspect is crowdstrike related, if I uv sync a project on a machine with crowdstrike it fails the first time, however, if I re try uv sync it succeeds. I thought this was maybe just me, but, I replicated it on 3 other machines (fails first attempt succeeds from the second onwards). 

Super weird!

---

_Comment by @Coruscant11 on 2025-04-02 18:29_

@DarrenPadraigDV Yes, I confirm that those issues are recurrent with Crowdstrike.
I suspect that the issue is that crowdstrike does not recognize uv as a well known tool. It easily blocks uv due to software packing behavior of uv which is the role of a package manager.
I don't know how can we reach them to fix that, but with uv becoming more and more popular, the issue tends to be more and more well known.

---
