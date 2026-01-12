```yaml
number: 8147
title: Ruff fails with memory allocation of X bytes failed
type: issue
state: closed
author: harpener
labels:
  - bug
  - cli
assignees: []
created_at: 2023-10-23T15:36:19Z
updated_at: 2024-02-14T14:09:22Z
url: https://github.com/astral-sh/ruff/issues/8147
synced_at: 2026-01-12T15:54:47Z
```

# Ruff fails with memory allocation of X bytes failed

---

_@harpener_

Hi guys,

for some time now, ruff has been failing with error "memory allocation of 7453010365139656813 bytes failed" if I run it for a specific file with option `--force-exclude`. Without it, is works fine, but includes excluded files for which it runs. It is working fine when checking the whole project, but I also use it among file watchers in Pycharm where it fails. I am using a custom config with some file exclusions, so in order to avoid ruff reports for excluded files, I need `--force-exclude` option. However, the file watcher has not been working properly for me, not from start, but for some time now, could be weeks or months. It only happens in one of my bigger projects, so it does not seem broken, maybe only demanding somehow. It does not bother me much, because it is so fast I can run it for the whole project instantly. :-)

Python: 3.8
Ruff: 0.0.292

Do you have any idea what could be causing this? Thanks


---

_Comment by @harpener on 2023-10-23 15:40_

I also just found out that using `--no-cache` option avoids the error, which is fine by me.

---

_Comment by @zanieb on 2023-10-23 16:30_

Hi! Is this with the `ruff check` command? Could you include an example full invocation of the command? Glad you've found a workaround in the meantime.

The stack trace from a debug build (e.g. clone the repository, `cargo build`, and use `./target/debug/ruff` with `RUST_BACKTRACE=1`) would be useful too.

---

_Comment by @harpener on 2023-10-23 16:39_

Hi, yeah, it is the check command, the full invocation looks like this:
`<path-to-ruff> --config <path-to-toml> --force-exclude --quiet <path-to-file>`

---

_Comment by @MichaReiser on 2023-10-23 23:56_

I noticed that yesterday too. It can happen if the caches are incompatible. I'm surprised you run into this because the ruff version is part of the cache key: Different ruff versions should never use the same cache. At least, that's how it's supposed to work. 

Can you try running `ruff clean`. This should fix your problem momentarily and isn't a long-term solution. 

Do I understand it correctly that you use a pre-built ruff version (you're not building ruff yourself)?

---

_Label `cli` added by @MichaReiser on 2023-10-23 23:57_

---

_Comment by @harpener on 2023-10-24 00:13_

Executing `ruff clean` helps as well. Now I can use cache again, against the same file, and it works. After the cache was built again, the file watcher still works for me. I am not sure how would the cache become corrupted on my side, I can come back if I notice it again, ideally with more info about what I did that could cause it. I always install new version using `pip`, I do not build it myself. Thanks for the help. Feel free to close the issue.

---

_Comment by @MichaReiser on 2023-10-24 01:00_

Thanks @harpener. Clad to hear that it helped. Yes, please come back if you run into the problem again. I'll keep this issue open for now to see if other users run into this problem as well.

---

_Comment by @phillipuniverse on 2023-10-26 05:09_

I also ran into this problem and `ruff clean` fixed it.

I was not switching between Ruff versions (all stables on ruff 0.1.2) but was running Ruff over and over. At some seemingly-random point I ran ruff and got the error:

```
poetry run ruff . --fix
memory allocation of 7453001542924727087 bytes failed
memory allocation of 8675468274863120500 bytes failed
make: *** [ruff] Abort trap: 6
```

---

_Label `bug` added by @MichaReiser on 2023-10-26 05:27_

---

_Comment by @charliermarsh on 2023-10-26 05:33_

Thank you for reporting -- we'll investigate.

---

_Comment by @MichaReiser on 2023-10-26 06:11_

I suspect that we read an incompatible cache for whatever reason, which makes `bincode` incorrectly interpret some byte sequence as the array length, resulting in it allocating a insanely large array

---

_Comment by @qarmin on 2023-10-27 11:05_

I had same problem with bincode 1.3.3, when I changed cache format(from storing BtreeMap<String, Item> to Vec<Item>)

This was "fixed" in 1.3.1 release - https://github.com/bincode-org/bincode/pull/336, but I can still reproduce problem with 1.3.3 version.

Maybe this is already fixed with 2.0.0 version, but not tested it yet 

---

_Comment by @qarmin on 2023-10-27 12:12_

New bincode issue - https://github.com/bincode-org/bincode/issues/676

---

_Comment by @qarmin on 2023-10-29 06:47_

So it is not really a bug in bincode, but by default it not limit allocation size, which may be really big in some situations(broken cache, changed save structure)

To fix problem, limits needs to be set - https://docs.rs/bincode/latest/bincode/config/trait.Options.html#method.with_limit and probably invalid cache files should be also deleted

---

_Comment by @MichaReiser on 2023-10-29 10:08_

Thanks.

I prefer identifying the root cause of this rather than hidding it with the size limit. Ruff shouldn't load incompstible caches, nor is it ever growing.

---

_Assigned to @MichaReiser by @MichaReiser on 2023-10-30 00:49_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-30 00:49_

---

_Comment by @MichaReiser on 2023-10-30 01:33_

Thins I tried without success so far:

* Run multiple instances of ruff at the same time, sporadically resetting the git cache (in the hope that they race on writing the cache)
* 

---

_Comment by @mrcljx on 2023-11-04 01:54_

I managed to capture a backtrace of the crash:

```
  21:        0x101b34604 - __rg_oom
                               at /rustc/a2f5f9691b6ce64c1703feaf9363710dfd7a56cf/library/std/src/alloc.rs:364:1
  22:        0x101bad1fc - alloc::alloc::handle_alloc_error::rt_error::h0b49241c647bd0ac
                               at /rustc/a2f5f9691b6ce64c1703feaf9363710dfd7a56cf/library/alloc/src/alloc.rs:383:13
  23:        0x101bad1fc - alloc::alloc::handle_alloc_error::h947fa5de62136ddf
                               at /rustc/a2f5f9691b6ce64c1703feaf9363710dfd7a56cf/library/alloc/src/alloc.rs:389:9
  24:        0x101a0e138 - alloc::raw_vec::handle_reserve::h68784d82265b35cc
                               at /rustc/a2f5f9691b6ce64c1703feaf9363710dfd7a56cf/library/alloc/src/raw_vec.rs:506:43
  25:        0x101ba9290 - alloc::raw_vec::RawVec<T,A>::reserve::do_reserve_and_handle::ha49ecf137cbb24a1
                               at /rustc/a2f5f9691b6ce64c1703feaf9363710dfd7a56cf/library/alloc/src/raw_vec.rs:289:13
  26:        0x101a0d504 - alloc::raw_vec::RawVec<T,A>::reserve::h99d157706d8f6389
                               at /rustc/a2f5f9691b6ce64c1703feaf9363710dfd7a56cf/library/alloc/src/raw_vec.rs:293:13
  27:        0x101a0d504 - alloc::vec::Vec<T,A>::reserve::h05ba621538aba417
                               at /rustc/a2f5f9691b6ce64c1703feaf9363710dfd7a56cf/library/alloc/src/vec/mod.rs:909:18
  28:        0x101a0a638 - alloc::vec::Vec<T,A>::extend_with::hc577c989726a2c72
                               at /rustc/a2f5f9691b6ce64c1703feaf9363710dfd7a56cf/library/alloc/src/vec/mod.rs:2534:9
  29:        0x101a0a96c - alloc::vec::Vec<T,A>::resize::hb21e03d42d0f7c1f
                               at /rustc/a2f5f9691b6ce64c1703feaf9363710dfd7a56cf/library/alloc/src/vec/mod.rs:2416:13
  30:        0x10073cc50 - bincode::de::read::IoReader<R>::fill_buffer::h1774c5c2f9bc7e8c
                               at ~/.cargo/registry/src/index.crates.io-6f17d22bba15001f/bincode-1.3.3/src/de/read.rs:144:9
  31:        0x10073cfa0 - <bincode::de::read::IoReader<R> as bincode::de::read::BincodeRead>::get_byte_buffer::h951d5d50670d2b39
                               at ~/.cargo/registry/src/index.crates.io-6f17d22bba15001f/bincode-1.3.3/src/de/read.rs:171:9
  32:        0x1006d8e00 - bincode::de::Deserializer<R,O>::read_vec::h17ca3b9d50b73b72
                               at ~/.cargo/registry/src/index.crates.io-6f17d22bba15001f/bincode-1.3.3/src/de/mod.rs:96:9
  33:        0x1006d8934 - bincode::de::Deserializer<R,O>::read_string::h51e7017758544173
                               at ~/.cargo/registry/src/index.crates.io-6f17d22bba15001f/bincode-1.3.3/src/de/mod.rs:100:19
  34:        0x1006da704 - <&mut bincode::de::Deserializer<R,O> as serde::de::Deserializer>::deserialize_string::hb3aa252b3728b54d
                               at ~/.cargo/registry/src/index.crates.io-6f17d22bba15001f/bincode-1.3.3/src/de/mod.rs:244:30
  35:        0x1006e9680 - serde::de::impls::<impl serde::de::Deserialize for alloc::string::String>::deserialize::hd78077d08aa2c9f6
                               at ~/.cargo/registry/src/index.crates.io-6f17d22bba15001f/serde-1.0.188/src/de/impls.rs:582:9
  36:        0x100682c8c - <core::marker::PhantomData<T> as serde::de::DeserializeSeed>::deserialize::h770e7118bcfe1463
                               at ~/.cargo/registry/src/index.crates.io-6f17d22bba15001f/serde-1.0.188/src/de/mod.rs:794:9
  37:        0x1006d6abc - <<&mut bincode::de::Deserializer<R,O> as serde::de::Deserializer>::deserialize_tuple::Access<R,O> as serde::de::SeqAccess>::next_element_seed::h7645625915be4900
                               at ~/.cargo/registry/src/index.crates.io-6f17d22bba15001f/bincode-1.3.3/src/de/mod.rs:314:25
  38:        0x1006d86ec - serde::de::SeqAccess::next_element::h93005d487dcc0a49
                               at ~/.cargo/registry/src/index.crates.io-6f17d22bba15001f/serde-1.0.188/src/de/mod.rs:1726:9
  39:        0x1006983f4 - <ruff_python_ast::imports::_::<impl serde::de::Deserialize for ruff_python_ast::imports::ModuleImport>::deserialize::__Visitor as serde::de::Visitor>::visit_seq::h8b7ad517c7ecda6b
                               at ~/ruff/crates/ruff_python_ast/src/imports.rs:123:49
```

Full trace here: https://gist.github.com/mrcljx/c0aa363ba32b16709d4d87a7c82278ae  -`RUST_BACKTRACE=1` did not work OOTB, because Rust doesn't panic on OOM, it just aborts, so I had to build a debug build with Rust nightly and use `set_alloc_error_hook`:

```
diff --git a/crates/ruff_cli/src/bin/ruff.rs b/crates/ruff_cli/src/bin/ruff.rs
index 81830ca25..8cdfb5f43 100644
--- a/crates/ruff_cli/src/bin/ruff.rs
+++ b/crates/ruff_cli/src/bin/ruff.rs
@@ -1,3 +1,4 @@
+#![feature(alloc_error_hook)]
 use std::process::ExitCode;
 
 use clap::{Parser, Subcommand};
@@ -6,6 +7,13 @@ use colored::Colorize;
 use ruff_cli::args::{Args, Command};
 use ruff_cli::{run, ExitStatus};
 
+
+use std::alloc::{Layout, set_alloc_error_hook};
+
+fn custom_alloc_error_hook(layout: Layout) {
+   panic!("memory allocation of {} bytes failed", layout.size());
+}
+
 #[cfg(target_os = "windows")]
 #[global_allocator]
 static GLOBAL: mimalloc::MiMalloc = mimalloc::MiMalloc;
@@ -23,6 +31,7 @@ static GLOBAL: mimalloc::MiMalloc = mimalloc::MiMalloc;
 static GLOBAL: tikv_jemallocator::Jemalloc = tikv_jemallocator::Jemalloc;
 
 pub fn main() -> ExitCode {
+    set_alloc_error_hook(custom_alloc_error_hook);
     let args = wild::args_os();
     let mut args =
         argfile::expand_args_from(args, argfile::parse_fromfile, argfile::PREFIX).unwrap();
```

---

_Comment by @MichaReiser on 2023-11-27 03:39_

@mrcljx oh nice, thanks for capturing the stacktrace. Unfortunately, I'm still surprised why it tries to load a version-incompatible cache. Or is it that we have a incompatible `Serialize` and `Deserialize` implementation somewhere where serializing and deserializing again fails? 

Anyone running into this issue and working on an open source project. Could you share the GitHub commit and upload your `.ruff-cache` directory. It would be a tremendous help fixing this issue.

---

_Comment by @0x26res on 2023-12-15 09:53_

I have the same issue, here's what my cache looks like:

```
ls -ltrh .ruff_cache/       
total 8
-rw-r--r--     1 xxx  staff    43B Mar  6  2023 CACHEDIR.TAG
drwxr-xr-x  1136 xxx  staff    36K Nov  7 14:30 content
drwxr-xr-x    48 xxx  staff   1.5K Nov  8 10:29 0.1.4
drwxr-xr-x    54 xxx  staff   1.7K Dec  1 11:51 0.1.6
```

I looked at file size, there's nothing out of the ordinary, the biggest file is 1.1mb and the total size is 10mb.


`--no-cache` fixes it. 


---

_Comment by @MichaReiser on 2023-12-15 10:50_

> I have the same issue, here's what my cache looks like:
> 
> ```
> ls -ltrh .ruff_cache/       
> total 8
> -rw-r--r--     1 xxx  staff    43B Mar  6  2023 CACHEDIR.TAG
> drwxr-xr-x  1136 xxx  staff    36K Nov  7 14:30 content
> drwxr-xr-x    48 xxx  staff   1.5K Nov  8 10:29 0.1.4
> drwxr-xr-x    54 xxx  staff   1.7K Dec  1 11:51 0.1.6
> ```
> 
> I looked at file size, there's nothing out of the ordinary, the biggest file is 1.1mb and the total size is 10mb.
> 
> `--no-cache` fixes it.

Is this happening in an open source project or a project where you would feel comfortable sharing the given repository state and cache directory content? It would help us analyse the issue.

---

_Comment by @0x26res on 2023-12-15 11:07_


> Is this happening in an open source project or a project where you would feel comfortable sharing the given repository state and cache directory content? It would help us analyse the issue.

Sadly I'm not comfortable sharing this information.

One thing that comes to mind is I have ruff running from 2 different virtual envs on the same repo:
- one runs from my dev environment, whenever I save file (that's the one that crashes)
- one runs from my pre-commit hook. It's potentially a different version of ruff, and python

maybe it could explain why the cache goes into a bad state.


---

_Comment by @johnthagen on 2023-12-18 17:34_

I started getting this issue with Ruff running as a File Watcher sorting imports in PyCharm (so it runs on single files each time use runs <kbd>ctrl+s</kbd>).

The Output:

```
/Users/User/Library/Caches/pypoetry/virtualenvs/app-TUB9piUY-py3.10/bin/python -m ruff check --select I --fix app.py
memory allocation of 8026310034279723890 bytes failed
```

```
$ ls -ltrh .ruff_cache/ 
total 8
-rw-r--r--     1 xxx  staff    43B May  1  2023 CACHEDIR.TAG
drwxr-xr-x  4227 xxx  staff   132K Oct 31 14:56 content
drwxr-xr-x    14 xxx  staff   448B Nov 30 12:48 0.1.6
drwxr-xr-x    14 xxx  staff   448B Dec  7 13:06 0.1.7
```

```
$ ruff --version
ruff 0.1.7
```

```
$ python --version
Python 3.10.13
```

Environment:

- macOS 13.6.1 x86

Code is not open source, so can't share unfortunately.

`ruff clean` fixed it.

---

_Comment by @tomhamiltonstubber on 2024-01-04 10:25_

Our team is working with a fairly large repo and we're getting the same issue.

Again, running `ruff clean` fixes the issue.

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2024-01-08 08:34_

---

_Unassigned @MichaReiser by @MichaReiser on 2024-01-08 08:34_

---

_Comment by @mattiabaldari on 2024-01-23 14:28_

I am also running into the same issue, but in my case `ruff clean` didn't solve it.
What solved in my case was running:
`rm -rf /path/to/project/.ruff_cache/0.1.*`

since inside `.ruff_cache` I had all of this:
`0.1.11       0.1.13       0.1.7        0.1.8        CACHEDIR.TAG`

---

_Comment by @dhruvmanila on 2024-01-24 15:09_

Interesting. The `ruff clean` command should be equivalent to what you ran as it basically removes the entire `.ruff_cache` directory. Did you have anything else apart from the 5 entries you've provided in the `.ruff_cache` directory i.e., cache directory from any other Ruff version?

---

_Comment by @mattiabaldari on 2024-01-24 15:16_

this is the entire output at the moment:
```shell
❯ ls /Users/mattia.baldari/project/.ruff_cache/
0.1.13       0.1.14       CACHEDIR.TAG
```

---

_Comment by @rumbarum on 2024-01-31 09:07_

I had encountered same error.
ls 
0.1.13       CACHEDIR.TAG

ruff 0.1.13
python 3.10.13
mac_os 14.1.2

My command, 
```
/opt/homebrew/bin/ruff --config ~/ruff.toml --fix  /path/to/repo/a-file.py
```
`ruff clean` not working
`rm -rf 0.1.13` not working
 `--no-cache` worked

I am using ruff  for globally not related for 1 project.
Any request is welcomed.

---

_Comment by @MichaReiser on 2024-01-31 09:33_

@rumbarum would you be able to share the repro file and the state of your cache directory so that we could try to reproduce locally?

---

_Comment by @rumbarum on 2024-01-31 10:39_

@MichaReiser 
Would you explain me what is and how to get repro file ?
And would you let me know how to get state of cache directory?



---

_Comment by @MichaReiser on 2024-02-01 11:19_

@rumbarum What would be useful for us is to have a project with a cache directory state that allows us to reproduce the error. What we need for this is:
* A link to the project or zip with the source (if publicly available, we can talk about different communication channels if you feel comfortable sharing the source to a closed source project)
* A zip with the content of the `.ruff_cache` directory



---

_Comment by @rumbarum on 2024-02-01 11:49_

@MichaReiser 
Currently, `.ruff_cache` is empty and no error.
I run ruff without --no-cache and gather caching. When error happen again I will touch you.

---

_Comment by @rumbarum on 2024-02-02 03:30_

@MichaReiser 
Error Happen again, and my repo is property of company. So it is not sharable.
But I can describe more detail,
Command on terminal works fine both on pycharm term or iterm2 without --no-cache.
This error happen only with pycharm File-watcher. This is my Pycharm setting.
<img width="1006" alt="image" src="https://github.com/astral-sh/ruff/assets/48576227/e0e02b0c-50a7-49b3-bf3d-37846f61e69d">
<img width="907" alt="image" src="https://github.com/astral-sh/ruff/assets/48576227/860f4078-6b79-4e94-9c87-5753d26b8989">

```
# on Filewatcher
/opt/homebrew/bin/ruff format --config ~/ruff.toml /Users/rumbarum/sig/slash-server/services/fastapi/app/views.py
memory allocation of 8751445353061442862 bytes failed

Process finished with exit code 134 (interrupted by signal 6:SIGABRT)
```

```
# on terminal
/opt/homebrew/bin/ruff format --config ~/ruff.toml /Users/rumbarum/sig/slash-server/services/fastapi/app/views.py                                   
1 file reformatted
```


---

_Comment by @MichaReiser on 2024-02-02 06:42_

Thanks @rumbarum I suspect that we run into some sort of concurrency issue that corrupts the cache. I'll play a bit with file watchers to see if I can corrupt my local cache.

---

_Comment by @rumbarum on 2024-02-02 07:43_

@MichaReiser 

FYI, my ruff config 
```
#  ~/ruff.toml 
line-length = 100
exclude = ["*/__init__.py"]

[lint]
# F=pyflakes
# I=Isort
select = ["F", "I"]

#F541 f-string without any placeholders
#F841 local variable '...' is assigned to but never used
#F403 from {name} import * used; unable to detect undefined names
#F401 '...' imported but unused --> Makefile (make ruff)에서만, 사용 강제합니다.
# ㄴ 작업 하면서 미리 import 하는 경우도 있기 때문에
ignore = ["F541", "F841", "F401", "F403"]
fixable = ["ALL"]

[isort]
relative-imports-order = "closest-to-furthest"

[format]
quote-style = "double"
indent-style = "space"
line-ending = "auto"
```

```
# pyproject.toml

[tool.ruff]
line-length = 100
exclude = [".venv", "./services/fastapi/migrations", "*/__init__.py"]

[tool.ruff.lint]
# F=pyflakes
# I=Isort
select = ["F", "I"]

#F541 f-string without any placeholders
#F841 local variable '...' is assigned to but never used
#F403 from {name} import * used; unable to detect undefined names
#F401 '...' imported but unused --> Makefile (make ruff)에서만, 사용 강제합니다.
# ㄴ 작업 하면서 미리 import 하는 경우도 있기 때문에
ignore = ["F541", "F841", "F401", "F403"]
fixable = ["ALL"]


[tool.ruff.lint.isort]
relative-imports-order = "closest-to-furthest"


[tool.ruff.format]
quote-style = "double"
indent-style = "space"
line-ending = "native"
exclude = [".venv", "./services/fastapi/migrations"]

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```
Ruff 0.1.3
Poetry (version 1.7.1),
PyCharm 2023.3.2 (Professional Edition)
M1 (14.1.2)

And If any code is not included in cache file (I only see paths), I will share current `.ruff_cache` through email. Let me know please.

---

_Comment by @GDYendell on 2024-02-06 17:00_

I currently have this error, so I am reporting here in the hope it is useful.

```
❯ ruff check
memory allocation of 3343762647483511827 bytes failed
Aborted (core dumped)
```

Running with `--no-cache` works fine.

I tried copying (`cp -R`) the project directory to see if I could reopen it in a new devcontainer and reproduce the error. In the new environment it worked fine, but it was using the version 0.2.1, whereas my existing environment was using 0.1.14. Updating my existing environment to 0.2.1 fixes the issue and rolling back to 0.1.14 breaks it again. Rolling back the new environment to 0.1.14 does not cause the issue. I also tried making another copy of the project and reopened in devcontainer with ruff pinned to 0.1.14, but this did not cause the problem either.

The project is open source. I have made a [branch](https://github.com/epics-containers/pvi/tree/sad-ruff-cache) from the checkout I currently see the issue in, including a zipped cache from the existing environment (sad) and the new environment (happy) in the root of the repo.

Something else that could might be relevant. In the current state of that branch, there is a formatting issue that black and ruff disagree on (`ruff format` disagrees with black, `ruff --fix` and `ruff check` are both happy with what black does, I think this is expected). So, maybe it is something to do with checking black formatting compatibility?

Given I can't even reproduce the error by copying the entire failing project directory I am not sure if checking out the project will help, but maybe the cache will be useful. I will leave the broken checkout untouched in case there are any other diagnostics that could be done with it.

---

_Comment by @MichaReiser on 2024-02-13 18:07_

Thanks @GDYendell . that's amazing. I tried to reproduce locally with ruff build from the v0.1.14 tag but failed to reproduce. I believe the reason is that our cache uses the project root as cache key. Would you mind sharing the absolute path of your project root? I think that should allow me to "force" ruff to use your cache data. If that doesn't work, then I probably have to look at the bincode files manually...  

I guess I can try to call decode on the files you provided. Let me give this a try

---

_Comment by @MichaReiser on 2024-02-13 21:11_

One possibility is that the `persist` method fails after writing some bytes. The file will be picked up during the next read. https://github.com/astral-sh/ruff/blob/b3dc5654731aa6a152b43a992c020c59bc416ce3/crates/ruff/src/cache.rs#L168-L176

We could try to delete the file when writing failed but there's still a risk that the process dies in the middle

---

_Comment by @GDYendell on 2024-02-13 22:28_

The absolute path to the broken checkout is `/scratch/development/pvi`.

---

_Comment by @MichaReiser on 2024-02-14 09:09_

I created [a small script](https://gist.github.com/MichaReiser/de919027fae9b2c6b668804c66157515) that deserializes the cache and prints the current offset. This can be useful to investigate where the file is corrupt.

The problematic cache file is `3276897697753128385`. The deserialization works fine up to the `ModuleImport` at offset 8144:

```
        module import entry 16@8144
[crates/ruff/tests/cache_issue.rs:199] self.read_len() = 21
          module: pvi.device.TextFormat
          range: 287..297
```

The next entry import entry (index 17) starts at offset 8181 but the length is only one byte instead of 8 (I can tell because what starts after is the name `typing.pvi.device.TextRead`)

```
@8144: 15 00 00 00 00 00 00 00 (length of name)
@8152: 70 76 69 2E 64 65 76 69 63 65 2E 54 65 78 74 46 6F 72 6D 61 74 (import name: pvi.device.TextFormat)
@8173: 1F 01 00 00 (range start) 
@8177: 29 01 00 00 (range end)
@8181: 13 74 79 70 69 6E 67 2E 70 76 69 2E 64 65 76 69 63 65 2E 54 65 78 74 52 65 61 64 (next module entry
@8181: typing.pvi.device.TextRead (the line above in text)
```

I suspect that we run into some form of race and the file only gets written partially. This is possible because we use a `BufWriter` that chunks the write calls. However, POSIX gives no guarantees that multiple `write` calls from the same process are atomic. It only guarantees that a single write call is atomic (to some extent). 

I think a first good starting point to fix this is to write the cache to a temporary file and then rename it when writing was successful. This doesn't just help with potential races but also protects about corrupted cache files in case the ruff process dies in the middle of writing the cache (for whatever reason).



---

_Comment by @MichaReiser on 2024-02-14 12:39_

Thank you all for your help and reproductions! Your reproductions and error reports allowed me to reproduce the bug locally and I now have a fix ready for review.

---

_Closed by @MichaReiser on 2024-02-14 14:09_

---
