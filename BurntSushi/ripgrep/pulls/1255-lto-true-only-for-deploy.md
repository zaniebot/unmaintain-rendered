```yaml
number: 1255
title: lto=true only for deploy
type: pull_request
state: closed
author: In-line
labels: []
assignees: []
base: master
head: patch-1
created_at: 2019-04-17T20:21:08Z
updated_at: 2019-06-24T15:04:31Z
url: https://github.com/BurntSushi/ripgrep/pull/1255
synced_at: 2026-01-12T18:23:13Z
```

# lto=true only for deploy

---

_@In-line_

_No description provided._

---

_Comment by @BurntSushi on 2019-04-17 20:23_

Do you  have any benchmarks justifying this change?

---

_Comment by @In-line on 2019-04-17 20:27_

@BurntSushi It was discussed many times before, as I undestood It is okay to enable LTO only on deploy. 

https://github.com/BurntSushi/ripgrep/pull/325#issuecomment-285069439

I will try to post some up to date benchmarks

---

_Comment by @BurntSushi on 2019-04-17 21:01_

I don't enable stuff like this just because it seems "okay." I'd like to have good justification for doing it.
My suspicion is that this will give a small but negligible performance benefit at the cost of a significantly longer release cycle, which I find frustrating. I'd also want to make sure that stack traces from panics still work correctly.

---

_@BurntSushi reviewed on 2019-04-17 21:02_

---

_Review comment by @BurntSushi on `ci/before_deploy.sh`:12 on 2019-04-17 21:02_

My guess is that this doesn't work on macOS.

---

_@okdana reviewed on 2019-04-17 21:07_

---

_Review comment by @okdana on `ci/before_deploy.sh`:12 on 2019-04-17 21:07_

It does not, both because of the missing `-i` extension and because the command can't be written that way. Something like this should work with both GNU and BSD `sed`:

```
sed -i.bak $'/\\[profile\.release\\]/a\\\nlto = true\n' Cargo.toml
```

The `grep` pattern is also not very precise

---

_Comment by @In-line on 2019-04-18 19:25_

Benchmarks without compiling linux kernel. It seems that thin lto is best in terms of performance for ripgrep.
```
# ./benchsuite --bench-iter=5 --warmup-iter=2 linux_literal
linux_literal (pattern: PM_RESUME)
----------------------------------
rg-lto-thin (ignore)          0.389 +/- 0.035 (lines: 17)
rg-lto-thin (ignore) (mmap)   0.898 +/- 0.022 (lines: 17)
rg-lto-thin (whitelist)*      0.354 +/- 0.025 (lines: 17)*

rg-lto-full (ignore)          0.394 +/- 0.031 (lines: 17)
rg-lto-full (ignore) (mmap)   0.907 +/- 0.030 (lines: 17)
rg-lto-full (whitelist)       0.389 +/- 0.025 (lines: 17)

rg-no-lto (ignore)            0.399 +/- 0.024 (lines: 17)
rg-no-lto (ignore) (mmap)     0.937 +/- 0.014 (lines: 17)
rg-no-lto (whitelist)         0.375 +/- 0.033 (lines: 17)
missing commands: ag, pt, sift, ucg, skipping benchmark linux_literal_casei (run with --allow-missing to run incomplete benchmarks)

linux_literal_default (pattern: PM_RESUME)
------------------------------------------
rg-lto-full   0.358 +/- 0.013 (lines: 17)*
rg-lto-thin*  0.380 +/- 0.035 (lines: 17)
rg-no-lto     0.421 +/- 0.044 (lines: 17)
```

---

_Comment by @In-line on 2019-04-18 19:41_

Same benchmark with `codegen-units=1`
```
linux_literal (pattern: PM_RESUME)
----------------------------------
rg-lto-thin (ignore)          0.342 +/- 0.029 (lines: 17)
rg-lto-thin (ignore) (mmap)   0.911 +/- 0.010 (lines: 17)
rg-lto-thin (whitelist)*      0.326 +/- 0.033 (lines: 17)

rg-lto-full (ignore)          0.357 +/- 0.037 (lines: 17)
rg-lto-full (ignore) (mmap)   0.881 +/- 0.022 (lines: 17)
rg-lto-full (whitelist)       0.321 +/- 0.015 (lines: 17)*

rg-no-lto (ignore)            0.393 +/- 0.014 (lines: 17)
rg-no-lto (ignore) (mmap)     0.883 +/- 0.043 (lines: 17)
rg-no-lto (whitelist)         0.346 +/- 0.008 (lines: 17)
missing commands: ag, pt, sift, ucg, skipping benchmark linux_literal_casei (run with --allow-missing to run incomplete benchmarks)

linux_literal_default (pattern: PM_RESUME)
------------------------------------------
rg-lto-full   0.355 +/- 0.013 (lines: 17)
rg-lto-thin*  0.364 +/- 0.019 (lines: 17)
rg-no-lto     0.352 +/- 0.016 (lines: 17)*
./benchsuite --bench-iter=5 --warmup-iter=
```

---

_Comment by @In-line on 2019-04-18 19:42_

@BurntSushi Is this good performance improvement to consider merging?

---

_Comment by @BurntSushi on 2019-04-19 13:57_

It looks pretty weak to be honest. I'm going to try playing with it locally to see how badly it impacts compilation times, and whether stack traces still work correctly. The last time I looked at, compilation times were pretty bad.

---

_Comment by @danilaml on 2019-06-19 11:38_

@BurntSushi Have you looked at the regular LTO or ThinLTO? ThinLTO should be pretty fast, at the cost of the slightly worse optimization (compared to the regular LTO).

---

_Comment by @BurntSushi on 2019-06-19 12:01_

@danilaml Sorry, but have you read the comments above? This entire PR is about enabling LTO or thin LTO, and even comes with benchmarks.

In any case, the benchmarks above show a very small gain. I just played around with compile times, and even enabling thin LTO results in about a ~33% longer incremental build. A build from scratch is only ~8% longer, which is perhaps tolerable, but a 33% longer incremental build is not.

So that means we could enable thin LTO in the release build, but I still don't think it's worth it, and would prefer to keep dogfooding the exact release configuration unless there's a more compelling motivation. The performance gains, as shown above, are middling at _best_. And the LTO binaries aren't even unambiguously faster in all cases.

---

_Closed by @BurntSushi on 2019-06-19 12:01_

---

_Comment by @In-line on 2019-06-24 15:04_

@BurntSushi Benchmarks are not full, because I don't have time to run absurd amount of time for benchmarks.

---
