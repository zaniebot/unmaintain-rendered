```yaml
number: 20845
title: Reduce size of WASM builds
type: issue
state: open
author: AbdealiLoKo
labels:
  - wish
assignees: []
created_at: 2025-10-13T14:21:53Z
updated_at: 2025-10-15T09:28:54Z
url: https://github.com/astral-sh/ruff/issues/20845
synced_at: 2026-01-10T11:09:59Z
```

# Reduce size of WASM builds

---

_Issue opened by @AbdealiLoKo on 2025-10-13 14:21_

### Question

Hi, I've been actively following and using the experimental version of ruff-wasm-web 
https://www.npmjs.com/package/@astral-sh/ruff-wasm-web

I noticed a large change in the wasm bundle size recently:
[0.9.0](https://www.npmjs.com/package/@astral-sh/ruff-wasm-web/v/0.9.0) -> 8.84 MB
[0.9.10](https://www.npmjs.com/package/@astral-sh/ruff-wasm-web/v/0.9.10) -> 9.06 MB
[0.10.0](https://www.npmjs.com/package/@astral-sh/ruff-wasm-web/v/0.10.0) -> 9.07 MB
[0.11.0](https://www.npmjs.com/package/@astral-sh/ruff-wasm-web/v/0.11.0) -> 9.06 MB
[0.11.13](https://www.npmjs.com/package/@astral-sh/ruff-wasm-web/v/0.11.13) -> 9.17 MB
[0.12.0](https://www.npmjs.com/package/@astral-sh/ruff-wasm-web/v/0.12.0) -> 9.23 MB
[0.12.4](https://www.npmjs.com/package/@astral-sh/ruff-wasm-web/v/0.12.4) -> 9.30 MB
[0.12.5](https://www.npmjs.com/package/@astral-sh/ruff-wasm-web/v/0.12.5) -> **12.8 MB**
[0.12.12](https://www.npmjs.com/package/@astral-sh/ruff-wasm-web/v/0.12.12) -> 13.3 MB
[0.13.0](https://www.npmjs.com/package/@astral-sh/ruff-wasm-web/v/0.13.0) -> 13.4 MB
[0.13.4](https://www.npmjs.com/package/@astral-sh/ruff-wasm-web/v/0.13.4) -> 13.9 MB
[0.14.0](https://www.npmjs.com/package/@astral-sh/ruff-wasm-web/v/0.14.0) -> 13.9 MB

8.84 MB -> 13.9 MB is nearly a 57% increase in bundle size

Just checking to see if this is expected ? For a browser, 8 MB is already considered large, and 14MB seemed to be surprising

Are there some suggestions or so to reduce the file size for the browser based wasm ?

### Version

_No response_

---

_Label `question` added by @AbdealiLoKo on 2025-10-13 14:21_

---

_Comment by @ntBre on 2025-10-13 14:39_

I only checked a few of these, but this roughly seems to match the trend in our release  binary size. We may need to take a look at this. Thanks for the report!

| Release | Size (MiB) |
|--------|--------|
| 0.9.0 | 37.4 |
| 0.12.0 | 43.3 |
| 0.14.0 | 51.3 | 

---

_Comment by @MichaReiser on 2025-10-13 17:35_

I suspect that this might be largely due to depending on `ty_python_semantic` for `ruff analyze`. We could consider gating the `analyze` dependency option but this tends to be very annoying. 


---

_Comment by @MichaReiser on 2025-10-13 17:38_

It seems that https://github.com/salsa-rs/salsa/pull/934 caused a significant size increase. Not necessarily what I would have expected, unless rustc is now able to do some more inlining. @ibraheemdev is this expected?

---

_Comment by @MichaReiser on 2025-10-14 12:37_

I asked claude to do some investigation and the finding is:


>  The 3MB size increase (20MB vs 17MB) is entirely due to symbol table bloat in the __LINKEDIT ?> segment:
> 
>   Key Findings:
>
>   1. Symbol count explosion:
>     - New binary: 46,133 symbols (3,079 exported)
>     - Old binary: 152 symbols (1 exported)
>   2. LINKEDIT segment growth:
>     - New binary: 3,965,680 bytes (~3.8 MB)
>     - Old binary: 404,816 bytes (~395 KB)
>     - Difference: ~3.56 MB (matches the total size increase)
>   3. The TEXT segment is actually smaller in the new binary (16.7MB vs 17MB), so the code itself hasn't grown.

These are all the newly exported symobls that don't get stripped even when using `stirp= "symbols"

https://gist.github.com/MichaReiser/2321249a9a4f71f185f84630fbe7d628

---

_Comment by @MichaReiser on 2025-10-14 12:53_

Using LTO="fat" reduces the binary by 3mb again

---

_Closed by @MichaReiser on 2025-10-15 07:00_

---

_Comment by @MichaReiser on 2025-10-15 09:28_

Turns out, that https://github.com/astral-sh/ruff/pull/20863 didn't help the WASM build (because WASM uses wasm-opt?). 

Optimizing the WASM binary size isn't a priority for us right now. But happy to accept any contribution that optimies our wasm build.

---

_Reopened by @MichaReiser on 2025-10-15 09:28_

---

_Label `question` removed by @MichaReiser on 2025-10-15 09:28_

---

_Label `wish` added by @MichaReiser on 2025-10-15 09:28_

---

_Renamed from "Size of ruff-wasm-web - increase by 57% over the past 9 months" to "Reduce size of WASM builds" by @MichaReiser on 2025-10-15 09:28_

---
