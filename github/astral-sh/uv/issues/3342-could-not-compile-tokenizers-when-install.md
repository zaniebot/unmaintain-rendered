---
number: 3342
title: "could not compile `tokenizers` when install transformers==4.30.2 in Python 3.12.1, casting `&T` to `&mut T` is undefined behavior"
type: issue
state: closed
author: Bewinxed
labels:
  - question
assignees: []
created_at: 2024-05-03T09:28:00Z
updated_at: 2024-05-03T09:30:19Z
url: https://github.com/astral-sh/uv/issues/3342
synced_at: 2026-01-07T13:12:17-06:00
---

# could not compile `tokenizers` when install transformers==4.30.2 in Python 3.12.1, casting `&T` to `&mut T` is undefined behavior

---

_Issue opened by @Bewinxed on 2024-05-03 09:28_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug. **uv pip install -r requirements.txt**
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag. **uv pip install -r requirements.txt on stable diffusion webui**
* The current uv platform. **Windows 11**
* The current uv version (`uv --version`). **uv 0.1.39 (028b40741 2024-04-27)**
-->

On stable diffusion webui
**uv pip install -r requirements.txt**
**Windows 11**
**Python 3.12.1**
**uv 0.1.39 (028b40741 2024-04-27)**
requirements.txt
```
GitPython
Pillow
accelerate

blendmodes
clean-fid
diskcache
einops
facexlib
fastapi>=0.90.1
gradio==3.41.2
inflection
jsonmerge
kornia
lark
numpy
omegaconf
open-clip-torch

piexif
psutil
pytorch_lightning
requests
resize-right

safetensors
scikit-image>=0.19
tomesd
torch
torchdiffeq
torchsde
transformers==4.30.2
pillow-avif-plugin==1.4.3
```

Error:
```
Running `rustc --crate-name tokenizers --edition=2018 tokenizers-lib\src/lib.rs --error-format=json --json=diagnostic-rendered-ansi,artifacts,future-incompat --crate-type lib --emit=dep-info,metadata,link -C opt-level=3 -C embed-bitcode=no --cfg "feature=\"cached-path\"" --cfg "feature=\"clap\"" --cfg "feature=\"cli\"" --cfg "feature=\"default\"" --cfg "feature=\"dirs\"" --cfg "feature=\"esaxx_fast\"" --cfg "feature=\"http\"" --cfg "feature=\"indicatif\"" --cfg "feature=\"onig\"" --cfg "feature=\"progressbar\"" --cfg "feature=\"reqwest\"" -C metadata=01f4626547be49f3 -C extra-filename=-01f4626547be49f3 --out-dir C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps -L dependency=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps --extern aho_corasick=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libaho_corasick-efbb8ecd5e3099c1.rmeta --extern cached_path=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libcached_path-c69e1d939c6d7c9e.rmeta --extern clap=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libclap-86f1acd5ec527307.rmeta --extern derive_builder=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libderive_builder-b903fc12a221a967.rmeta --extern dirs=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libdirs-5830ada473ccc337.rmeta --extern esaxx_rs=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libesaxx_rs-9f71f8cb7f9a1e61.rmeta --extern getrandom=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libgetrandom-dd7dcad96763f6a7.rmeta --extern indicatif=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libindicatif-c964f1ee2c9318a9.rmeta --extern itertools=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libitertools-cf00460d62ceff2e.rmeta --extern lazy_static=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\liblazy_static-bfdec79560375188.rmeta --extern log=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\liblog-5b0634833b312a22.rmeta --extern macro_rules_attribute=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libmacro_rules_attribute-4470de80f56ebaeb.rmeta --extern monostate=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libmonostate-8e6265d753fc42a7.rmeta --extern onig=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libonig-02a798b81f126e24.rmeta --extern paste=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\paste-61028428a2d80c90.dll --extern rand=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\librand-2323406285ed9085.rmeta --extern rayon=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\librayon-3992211298425b89.rmeta --extern rayon_cond=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\librayon_cond-a5a2cb423c727fd0.rmeta --extern regex=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libregex-a3cce07d18a1de78.rmeta --extern regex_syntax=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libregex_syntax-120493d0297a522c.rmeta --extern reqwest=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libreqwest-305ec3aa7cdb6762.rmeta --extern serde=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libserde-2771a41a2dc22fbe.rmeta --extern serde_json=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libserde_json-bd5b553725a3f599.rmeta --extern spm_precompiled=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libspm_precompiled-647ff93c8b219486.rmeta --extern thiserror=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libthiserror-b6123d0cf5f7f4de.rmeta --extern unicode_normalization_alignments=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libunicode_normalization_alignments-93b446b2a89d3fb2.rmeta --extern unicode_segmentation=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libunicode_segmentation-0e6d9268c6126518.rmeta --extern unicode_categories=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libunicode_categories-a22859ed816b6d05.rmeta -L native=C:\Users\Bewinxed\.cargo\registry\src\index.crates.io-6f17d22bba15001f\windows_x86_64_msvc-0.52.5\lib -L native=C:\Users\Bewinxed\.cargo\registry\src\index.crates.io-6f17d22bba15001f\windows_x86_64_msvc-0.48.5\lib -L "native=C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.39.33519\atlmfc\lib\x64" -L native=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\build\bzip2-sys-1bf912515f16ba38\out\lib -L "native=C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.39.33519\atlmfc\lib\x64" -L native=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\build\zstd-sys-0cf4a3fed15689c9\out -L "native=C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.39.33519\atlmfc\lib\x64" -L native=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\build\esaxx-rs-47f3108e3cc4c7b6\out -L "native=C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.39.33519\atlmfc\lib\x64" -L native=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\build\onig_sys-d1f90d6ca3873404\out`
warning: variable does not need to be mutable
   --> tokenizers-lib\src\models\unigram\model.rs:265:21
    |
265 |                 let mut target_node = &mut best_path_ends_at[key_pos];
    |                     ----^^^^^^^^^^^
    |                     |
    |                     help: remove this `mut`
    |
    = note: `#[warn(unused_mut)]` on by default

warning: variable does not need to be mutable
   --> tokenizers-lib\src\models\unigram\model.rs:282:21
    |
282 |                 let mut target_node = &mut best_path_ends_at[starts_at + mblen];
    |                     ----^^^^^^^^^^^
    |                     |
    |                     help: remove this `mut`

warning: variable does not need to be mutable
   --> tokenizers-lib\src\pre_tokenizers\byte_level.rs:200:59
    |
200 |     encoding.process_tokens_with_offsets_mut(|(i, (token, mut offsets))| {
    |                                                           ----^^^^^^^
    |                                                           |
    |                                                           help: remove this `mut`

error: casting `&T` to `&mut T` is undefined behavior, even if the reference is unused, consider instead using an `UnsafeCell`
   --> tokenizers-lib\src\models\bpe\trainer.rs:526:47
    |
522 |                     let w = &words[*i] as *const _ as *mut _;
    |                             -------------------------------- casting happend here
...
526 |                         let word: &mut Word = &mut (*w);
    |                                               ^^^^^^^^^
    |
    = note: for more information, visit <https://doc.rust-lang.org/book/ch15-05-interior-mutability.html>
    = note: `#[deny(invalid_reference_casting)]` on by default

warning: `tokenizers` (lib) generated 3 warnings
error: could not compile `tokenizers` (lib) due to 1 previous error; 3 warnings emitted

Caused by:
  process didn't exit successfully: `rustc --crate-name tokenizers --edition=2018 tokenizers-lib\src/lib.rs --error-format=json --json=diagnostic-rendered-ansi,artifacts,future-incompat --crate-type lib --emit=dep-info,metadata,link -C opt-level=3 -C embed-bitcode=no --cfg "feature=\"cached-path\"" --cfg "feature=\"clap\"" --cfg "feature=\"cli\"" --cfg "feature=\"default\"" --cfg "feature=\"dirs\"" --cfg "feature=\"esaxx_fast\"" --cfg "feature=\"http\"" --cfg "feature=\"indicatif\"" --cfg "feature=\"onig\"" --cfg "feature=\"progressbar\"" --cfg "feature=\"reqwest\"" -C metadata=01f4626547be49f3 -C extra-filename=-01f4626547be49f3 --out-dir C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps -L dependency=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps --extern aho_corasick=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libaho_corasick-efbb8ecd5e3099c1.rmeta --extern cached_path=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libcached_path-c69e1d939c6d7c9e.rmeta --extern clap=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libclap-86f1acd5ec527307.rmeta --extern derive_builder=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libderive_builder-b903fc12a221a967.rmeta --extern dirs=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libdirs-5830ada473ccc337.rmeta --extern esaxx_rs=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libesaxx_rs-9f71f8cb7f9a1e61.rmeta --extern getrandom=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libgetrandom-dd7dcad96763f6a7.rmeta --extern indicatif=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libindicatif-c964f1ee2c9318a9.rmeta --extern itertools=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libitertools-cf00460d62ceff2e.rmeta --extern lazy_static=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\liblazy_static-bfdec79560375188.rmeta --extern log=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\liblog-5b0634833b312a22.rmeta --extern macro_rules_attribute=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libmacro_rules_attribute-4470de80f56ebaeb.rmeta --extern monostate=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libmonostate-8e6265d753fc42a7.rmeta --extern onig=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libonig-02a798b81f126e24.rmeta --extern paste=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\paste-61028428a2d80c90.dll --extern rand=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\librand-2323406285ed9085.rmeta --extern rayon=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\librayon-3992211298425b89.rmeta --extern rayon_cond=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\librayon_cond-a5a2cb423c727fd0.rmeta --extern regex=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libregex-a3cce07d18a1de78.rmeta --extern regex_syntax=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libregex_syntax-120493d0297a522c.rmeta --extern reqwest=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libreqwest-305ec3aa7cdb6762.rmeta --extern serde=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libserde-2771a41a2dc22fbe.rmeta --extern serde_json=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libserde_json-bd5b553725a3f599.rmeta --extern spm_precompiled=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libspm_precompiled-647ff93c8b219486.rmeta --extern thiserror=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libthiserror-b6123d0cf5f7f4de.rmeta --extern unicode_normalization_alignments=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libunicode_normalization_alignments-93b446b2a89d3fb2.rmeta --extern unicode_segmentation=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libunicode_segmentation-0e6d9268c6126518.rmeta --extern unicode_categories=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\deps\libunicode_categories-a22859ed816b6d05.rmeta -L native=C:\Users\Bewinxed\.cargo\registry\src\index.crates.io-6f17d22bba15001f\windows_x86_64_msvc-0.52.5\lib -L native=C:\Users\Bewinxed\.cargo\registry\src\index.crates.io-6f17d22bba15001f\windows_x86_64_msvc-0.48.5\lib -L "native=C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.39.33519\atlmfc\lib\x64" -L native=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\build\bzip2-sys-1bf912515f16ba38\out\lib -L "native=C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.39.33519\atlmfc\lib\x64" -L native=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\build\zstd-sys-0cf4a3fed15689c9\out -L "native=C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.39.33519\atlmfc\lib\x64" -L native=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\build\esaxx-rs-47f3108e3cc4c7b6\out -L "native=C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.39.33519\atlmfc\lib\x64" -L native=C:\Users\Bewinxed\AppData\Local\uv\cache\built-wheels-v3\pypi\tokenizers\0.13.3\RSd8TDfK9pFoy90VRjtQY\tokenizers-0.13.3.tar.gz\target\release\build\onig_sys-d1f90d6ca3873404\out` (exit code: 1)
warning: build failed, waiting for other jobs to finish...
error: `cargo rustc --lib --message-format=json-render-diagnostics --manifest-path Cargo.toml --release -v --features pyo3/extension-module --crate-type cdylib --` failed with code 101
```

---

_Renamed from "could not compile `tokenizers` casting `&T` to `&mut T` is undefined behavior" to "could not compile `tokenizers` when install transformers==4.30.2 in Python 3.12.1, casting `&T` to `&mut T` is undefined behavior" by @Bewinxed on 2024-05-03 09:29_

---

_Comment by @konstin on 2024-05-03 09:30_

This is a problem in `tokenizers`, not in uv (https://github.com/huggingface/tokenizers/issues/1485).

---

_Closed by @konstin on 2024-05-03 09:30_

---

_Label `question` added by @konstin on 2024-05-03 09:30_

---
