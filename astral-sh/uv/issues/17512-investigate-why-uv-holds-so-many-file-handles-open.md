```yaml
number: 17512
title: Investigate why uv holds so many file handles open
type: issue
state: open
author: konstin
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2026-01-16T09:32:03Z
updated_at: 2026-01-20T22:55:44Z
url: https://github.com/astral-sh/uv/issues/17512
synced_at: 2026-01-20T23:51:49Z
```

# Investigate why uv holds so many file handles open

---

_@konstin_

We got reports that uv overruns the default open files limit (ulimit) for users on Linux and macOS:

* During sync/building packages: https://github.com/astral-sh/uv/issues/11296
* During bytecode compilation: https://github.com/astral-sh/uv/issues/16999
* When uninstall Python versions (no GitHub issue yet)

The default ulimits can be low, for example 1024 on linux, and we spawn up to a thread per core and several subprocesses. However, Cargo and other Rust tools which have very similar workloads and often very similar problems to us, don't have any problems with ulimits. A cargo maintainer [told us](https://rust-lang.zulipchat.com/#narrow/channel/246057-t-cargo/topic/Do.20cargo.2Frustc.20handle.20low.20ulimits.3F/near/568121917):

> From my testing for fine grain locking (unstable feature), I found that Cargo generally peaks with around ~70-80 file descriptors opened at once, and this remainly mostly static even for large projects.

So the question is, why are we running into the ulimit, while Cargo doesn't? What file descriptors is uv even holding, do we need them? Is Cargo doing something different that we can adopt to avoid this?

This requires some Unix expertise and rust parallelism knowledge, but should otherwise not require much uv specific details, this is mostly around figuring out how to analyze this this in uv and determining the differences to Cargo.

---

_Label `bug` added by @konstin on 2026-01-16 09:32_

---

_Label `good first issue` added by @konstin on 2026-01-16 09:32_

---

_Label `help wanted` added by @konstin on 2026-01-16 09:32_

---

_Comment by @chilin0525 on 2026-01-17 07:25_

I can reproduce this on macOS with the following steps:
```bash
git clone https://github.com/apache/airflow
cd airflow
uv sync
```

Output:
```bash
Resolved 919 packages in 147ms
  × Failed to build `apache-airflow-providers-apache-beam @ file:///Users/chilin/airflow/providers/apache/beam`
  ├─▶ Failed to write to the distribution cache
  ╰─▶ Too many open files (os error 24) at path "/Users/chilin/.cache/uv/sdists-v9/editable/e2cbf38bf7b0039e/.tmpFEZOEh"
  help: `apache-airflow-providers-apache-beam` was included because `apache-airflow[all]` (v3.2.0) depends on `apache-airflow-providers-apache-beam`
```

To investigate further, I monitored the file descriptors opened by `uv` using `lsof`. It appears uv opens a large number of lock/temp files under the distribution cache:
```bash
COMMAND   PID   USER   FD      TYPE             DEVICE  SIZE/OFF      NODE NAME
uv      16284 chilin  cwd       DIR               1,17      2432 880274528 /Users/chilin/airflow
uv      16284 chilin  txt       REG               1,17  42561408 880290718 /opt/homebrew/Cellar/uv/0.9.26/bin/uv
uv      16284 chilin  txt       REG               1,17     64404 871574996 /Library/Preferences/Logging/.plist-cache.wYlFUyJH
uv      16284 chilin    0u      CHR               16,7 0t1085745       849 /dev/ttys007
uv      16284 chilin    1u      CHR               16,7 0t1085745       849 /dev/ttys007
uv      16284 chilin    2u      CHR               16,7 0t1085745       849 /dev/ttys007
uv      16284 chilin    3u   KQUEUE                                        count=1, state=0x8
uv      16284 chilin    4u   KQUEUE                                        count=1, state=0x8
uv      16284 chilin    5u   KQUEUE                                        count=1, state=0x8
uv      16284 chilin    6u     unix 0xf24d7a68e1476231       0t0           ->0x9e0c87d8248b492f
uv      16284 chilin    7u     unix 0x9e0c87d8248b492f       0t0           ->0xf24d7a68e1476231
uv      16284 chilin    8u     unix 0xf24d7a68e1476231       0t0           ->0x9e0c87d8248b492f
uv      16284 chilin    9u      REG               1,17         0 880623121 /Users/chilin/.cache/uv/.lock
uv      16284 chilin   10u      REG               1,17         0 880291060 /Users/chilin/airflow/.venv/.lock
uv      16284 chilin   11u      REG               1,17         0 880687136 /Users/chilin/.cache/uv/sdists-v9/editable/ca01cee3cc779a8a/.lock
uv      16284 chilin   12u      REG               1,17         0 880687138 /Users/chilin/.cache/uv/sdists-v9/editable/4b31918dae43f06c/.lock
uv      16284 chilin   13   NPOLICY
uv      16284 chilin   14u    systm 0xc0cad64333db6606       0t0           [ctl com.apple.netsrc id 7 unit 40]
uv      16284 chilin   15u     unix 0xc2f395d1b0476ac8       0t0           ->0x57b70c755d534344
uv      16284 chilin   16u      REG               1,17         0 880687140 /Users/chilin/.cache/uv/sdists-v9/editable/c769b8d16ab58fda/.lock
uv      16284 chilin   17u      REG               1,17         0 880687142 /Users/chilin/.cache/uv/sdists-v9/editable/87b7f3a52a3b3cd1/.lock
uv      16284 chilin   18u      REG               1,17         0 880687144 /Users/chilin/.cache/uv/sdists-v9/editable/fcd35907e56912c0/.lock
uv      16284 chilin   19u      REG               1,17         0 880687146 /Users/chilin/.cache/uv/sdists-v9/editable/5ef5ee08fa7a9d30/.lock
uv      16284 chilin   20u      REG               1,17         0 880687148 /Users/chilin/.cache/uv/sdists-v9/editable/b027303c973a6ded/.lock
uv      16284 chilin   21u      REG               1,17         0 880623136 /Users/chilin/.cache/uv/sdists-v9/editable/182bc3e5fd1eda15/.lock
uv      16284 chilin   22u      REG               1,17         0 880687150 /Users/chilin/.cache/uv/sdists-v9/editable/6c7aba01d507a0c3/.lock
uv      16284 chilin   23u      REG               1,17         0 880687152 /Users/chilin/.cache/uv/sdists-v9/editable/49a7a1890348b488/.lock
uv      16284 chilin   24u      REG               1,17         0 880687154 /Users/chilin/.cache/uv/sdists-v9/editable/ccba3c35f8f0b91a/.lock
uv      16284 chilin   25u      REG               1,17         0 880687156 /Users/chilin/.cache/uv/sdists-v9/editable/1dbe6eedc2151d7f/.lock
uv      16284 chilin   26u      REG               1,17         0 880687158 /Users/chilin/.cache/uv/sdists-v9/editable/323c8fd3805fbb08/.lock
uv      16284 chilin   27u      REG               1,17         0 880687160 /Users/chilin/.cache/uv/sdists-v9/editable/bc7d1e2449028c10/.lock
uv      16284 chilin   28u      REG               1,17         0 880687162 /Users/chilin/.cache/uv/sdists-v9/editable/6d22594f6cc8e272/.lock
uv      16284 chilin   29u      REG               1,17         0 880687164 /Users/chilin/.cache/uv/sdists-v9/editable/88a7f61c25e41cd1/.lock
uv      16284 chilin   30u      REG               1,17         0 880623138 /Users/chilin/.cache/uv/sdists-v9/editable/875a08219b3937e4/.lock
uv      16284 chilin   31u      REG               1,17         0 880687166 /Users/chilin/.cache/uv/sdists-v9/editable/3422ad5e1802674f/.lock
uv      16284 chilin   32u      REG               1,17         0 880687168 /Users/chilin/.cache/uv/sdists-v9/editable/922116ff863388d5/.lock
uv      16284 chilin   33u      REG               1,17         0 880687170 /Users/chilin/.cache/uv/sdists-v9/editable/5a5430d2f9ff93cd/.lock
uv      16284 chilin   34u      REG               1,17         0 880687172 /Users/chilin/.cache/uv/sdists-v9/editable/f8a74177cae05b5c/.lock
uv      16284 chilin   35u      REG               1,17         0 880687174 /Users/chilin/.cache/uv/sdists-v9/editable/ea53d3f1f80b816d/.lock
uv      16284 chilin   36u      REG               1,17         0 880687176 /Users/chilin/.cache/uv/sdists-v9/editable/e2cbf38bf7b0039e/.lock
uv      16284 chilin   37u      REG               1,17         0 880687178 /Users/chilin/.cache/uv/sdists-v9/editable/25fab1917f5db535/.lock
uv      16284 chilin   38u      REG               1,17         0 880687180 /Users/chilin/.cache/uv/sdists-v9/editable/d54731290b88626e/.lock
uv      16284 chilin   39u      REG               1,17         0 880687182 /Users/chilin/.cache/uv/sdists-v9/editable/c0e6e5d2542349bb/.lock
uv      16284 chilin   40u      REG               1,17         0 880687184 /Users/chilin/.cache/uv/sdists-v9/editable/1040a9214d697b6e/.lock
uv      16284 chilin   41u      REG               1,17         0 880687186 /Users/chilin/.cache/uv/sdists-v9/editable/43c3db7d342f7e05/.lock
uv      16284 chilin   42u      REG               1,17         0 880687188 /Users/chilin/.cache/uv/sdists-v9/editable/b6482d287827c7b7/.lock
uv      16284 chilin   43u      REG               1,17         0 880687190 /Users/chilin/.cache/uv/sdists-v9/editable/aeada9c008cb7936/.lock
uv      16284 chilin   44u      REG               1,17         0 880687192 /Users/chilin/.cache/uv/sdists-v9/editable/9af382879adce12d/.lock
uv      16284 chilin   45u      REG               1,17         0 880687194 /Users/chilin/.cache/uv/sdists-v9/editable/97b792d43ac2dbd1/.lock
uv      16284 chilin   46u      REG               1,17         0 880687196 /Users/chilin/.cache/uv/sdists-v9/editable/ae4288485a7fcc43/.lock
uv      16284 chilin   47u      REG               1,17         0 880687198 /Users/chilin/.cache/uv/sdists-v9/editable/693da44fd719edde/.lock
uv      16284 chilin   48u      REG               1,17         0 880687200 /Users/chilin/.cache/uv/sdists-v9/editable/2891c8a1c1633df4/.lock
uv      16284 chilin   49u      REG               1,17         0 880687202 /Users/chilin/.cache/uv/sdists-v9/editable/3536dbc9a49cdcb2/.lock
uv      16284 chilin   50u      REG               1,17         0 880687204 /Users/chilin/.cache/uv/sdists-v9/editable/4af3993f6f74cfbc/.lock
uv      16284 chilin   51u      REG               1,17         0 880687206 /Users/chilin/.cache/uv/sdists-v9/editable/ecd5793782f0e648/.lock
uv      16284 chilin   52u      REG               1,17         0 880687208 /Users/chilin/.cache/uv/sdists-v9/editable/533d597557aa87ba/.lock
uv      16284 chilin   53u      REG               1,17         0 880687210 /Users/chilin/.cache/uv/sdists-v9/editable/3d6ed35fbd354a2b/.lock
uv      16284 chilin   54u      REG               1,17         0 880687212 /Users/chilin/.cache/uv/sdists-v9/editable/0e3f7fdca55103df/.lock
uv      16284 chilin   55u      REG               1,17         0 880687214 /Users/chilin/.cache/uv/sdists-v9/editable/f0aa564ac9a98583/.lock
uv      16284 chilin   56u      REG               1,17         0 880687216 /Users/chilin/.cache/uv/sdists-v9/editable/9aa4184969afffcf/.lock
uv      16284 chilin   57u      REG               1,17         0 880687218 /Users/chilin/.cache/uv/sdists-v9/editable/7bddf2532f579094/.lock
uv      16284 chilin   58u      REG               1,17         0 880687220 /Users/chilin/.cache/uv/sdists-v9/editable/1a762b48f81c3448/.lock
uv      16284 chilin   59u      REG               1,17         0 880687222 /Users/chilin/.cache/uv/sdists-v9/editable/2db040ca9caad73e/.lock
uv      16284 chilin   60u      REG               1,17         0 880687224 /Users/chilin/.cache/uv/sdists-v9/editable/928d5ebe4e5419e3/.lock
uv      16284 chilin   61u      REG               1,17         0 880687226 /Users/chilin/.cache/uv/sdists-v9/editable/9d01b16f566c7da9/.lock
uv      16284 chilin   62u      REG               1,17         0 880687228 /Users/chilin/.cache/uv/sdists-v9/editable/7e316389f6b38ec8/.lock
uv      16284 chilin   63u      REG               1,17         0 880687230 /Users/chilin/.cache/uv/sdists-v9/editable/24d5ace8797c5911/.lock
uv      16284 chilin   64u      REG               1,17         0 880687232 /Users/chilin/.cache/uv/sdists-v9/editable/22f4e6c188b59bd2/.lock
uv      16284 chilin   65u      REG               1,17         0 880687234 /Users/chilin/.cache/uv/sdists-v9/editable/c228f974ef24262a/.lock
uv      16284 chilin   66u      REG               1,17         0 880687236 /Users/chilin/.cache/uv/sdists-v9/editable/60d4db9fee08f281/.lock
uv      16284 chilin   67u      REG               1,17         0 880687238 /Users/chilin/.cache/uv/sdists-v9/editable/a3e97f579e6f5ff6/.lock
uv      16284 chilin   68u      REG               1,17         0 880687240 /Users/chilin/.cache/uv/sdists-v9/editable/41ed3a5ec67387f7/.lock
uv      16284 chilin   69u      REG               1,17         0 880687242 /Users/chilin/.cache/uv/sdists-v9/editable/1bde0f6d17c7caac/.lock
uv      16284 chilin   70u      REG               1,17         0 880687244 /Users/chilin/.cache/uv/sdists-v9/editable/ec407ddceb3c67c8/.lock
uv      16284 chilin   71u      REG               1,17         0 880687246 /Users/chilin/.cache/uv/sdists-v9/editable/fa49263368763d09/.lock
uv      16284 chilin   72u      REG               1,17         0 880687248 /Users/chilin/.cache/uv/sdists-v9/editable/163e460db77d1de5/.lock
uv      16284 chilin   73u      REG               1,17         0 880687250 /Users/chilin/.cache/uv/sdists-v9/editable/430cb57e7bf98a37/.lock
uv      16284 chilin   74u      REG               1,17         0 880687252 /Users/chilin/.cache/uv/sdists-v9/editable/f09c4bc21712699d/.lock
uv      16284 chilin   75u      REG               1,17         0 880687254 /Users/chilin/.cache/uv/sdists-v9/editable/3507871140add3f8/.lock
uv      16284 chilin   76u      REG               1,17         0 880687256 /Users/chilin/.cache/uv/sdists-v9/editable/579d605d5e5bcc48/.lock
uv      16284 chilin   77u      REG               1,17         0 880687258 /Users/chilin/.cache/uv/sdists-v9/editable/6f145954ec347507/.lock
uv      16284 chilin   78u      REG               1,17         0 880687260 /Users/chilin/.cache/uv/sdists-v9/editable/bec8c5d029349f41/.lock
uv      16284 chilin   79u      REG               1,17         0 880687262 /Users/chilin/.cache/uv/sdists-v9/editable/ea66f1bb8b7762e7/.lock
uv      16284 chilin   80u      REG               1,17         0 880687264 /Users/chilin/.cache/uv/sdists-v9/editable/bed7ee496780cc92/.lock
uv      16284 chilin   81u      REG               1,17         0 880687266 /Users/chilin/.cache/uv/sdists-v9/editable/ca628033769f37e4/.lock
uv      16284 chilin   82u      REG               1,17         0 880687268 /Users/chilin/.cache/uv/sdists-v9/editable/919111bf74ec2954/.lock
uv      16284 chilin   83u      REG               1,17         0 880687270 /Users/chilin/.cache/uv/sdists-v9/editable/a5388dc090f7c065/.lock
uv      16284 chilin   84u      REG               1,17         0 880687272 /Users/chilin/.cache/uv/sdists-v9/editable/0f9b3789084f4140/.lock
uv      16284 chilin   85u      REG               1,17         0 880687274 /Users/chilin/.cache/uv/sdists-v9/editable/c7f58aeb38f12970/.lock
uv      16284 chilin   86u      REG               1,17         0 880687276 /Users/chilin/.cache/uv/sdists-v9/editable/beb8639d405c5167/.lock
uv      16284 chilin   87u      REG               1,17         0 880687278 /Users/chilin/.cache/uv/sdists-v9/editable/52c2e5b0a3abbb16/.lock
uv      16284 chilin   88u      REG               1,17         0 880687280 /Users/chilin/.cache/uv/sdists-v9/editable/400f8eaf970a722f/.lock
uv      16284 chilin   89u      REG               1,17         0 880687282 /Users/chilin/.cache/uv/sdists-v9/editable/25bdd0218e36e538/.lock
uv      16284 chilin   90u      REG               1,17         0 880687284 /Users/chilin/.cache/uv/sdists-v9/editable/438a8d84c97c49b5/.lock
uv      16284 chilin   91u      REG               1,17         0 880687286 /Users/chilin/.cache/uv/sdists-v9/editable/59e4484d2192c943/.lock
uv      16284 chilin   92u      REG               1,17         0 880687288 /Users/chilin/.cache/uv/sdists-v9/editable/693c9708da666834/.lock
uv      16284 chilin   93u      REG               1,17         0 880687290 /Users/chilin/.cache/uv/sdists-v9/editable/b7bc15d8443ae061/.lock
uv      16284 chilin   94u      REG               1,17         0 880687292 /Users/chilin/.cache/uv/sdists-v9/editable/dcdd9d2597d62f56/.lock
uv      16284 chilin   95u      REG               1,17         0 880687294 /Users/chilin/.cache/uv/sdists-v9/editable/e5204c77b346d1f5/.lock
uv      16284 chilin   96u      REG               1,17         0 880687296 /Users/chilin/.cache/uv/sdists-v9/editable/82ad1b230e7233cf/.lock
uv      16284 chilin   97u      REG               1,17         0 880687298 /Users/chilin/.cache/uv/sdists-v9/editable/daffd7fb1938b330/.lock
uv      16284 chilin   98u      REG               1,17         0 880687300 /Users/chilin/.cache/uv/sdists-v9/editable/9ec75ecd4fa64456/.lock
uv      16284 chilin   99u      REG               1,17         0 880687302 /Users/chilin/.cache/uv/sdists-v9/editable/5c6b97221a82e4b6/.lock
uv      16284 chilin  100u      REG               1,17         0 880687304 /Users/chilin/.cache/uv/sdists-v9/editable/16d841667ef486b4/.lock
uv      16284 chilin  101u      REG               1,17         0 880687306 /Users/chilin/.cache/uv/sdists-v9/editable/fed976fbf9c8a2df/.lock
uv      16284 chilin  102u      REG               1,17         0 880687308 /Users/chilin/.cache/uv/sdists-v9/editable/83f3be9a0f4ecc49/.lock
uv      16284 chilin  103u      REG               1,17         0 880687310 /Users/chilin/.cache/uv/sdists-v9/editable/45743f877212ec85/.lock
uv      16284 chilin  104u      REG               1,17         0 880687312 /Users/chilin/.cache/uv/sdists-v9/editable/99ba3ebb89790df6/.lock
uv      16284 chilin  105u      REG               1,17         0 880687314 /Users/chilin/.cache/uv/sdists-v9/editable/d3d2d296c6d36f60/.lock
uv      16284 chilin  106u      REG               1,17         0 880687316 /Users/chilin/.cache/uv/sdists-v9/editable/e23b45b1cc746bfc/.lock
uv      16284 chilin  107u      REG               1,17         0 880687318 /Users/chilin/.cache/uv/sdists-v9/editable/81ed83f02afeccd6/.lock
uv      16284 chilin  108u      REG               1,17         0 880687320 /Users/chilin/.cache/uv/sdists-v9/editable/59b3fc416f552f8a/.lock
uv      16284 chilin  109u      REG               1,17         0 880687322 /Users/chilin/.cache/uv/sdists-v9/editable/97ac4ac2c82bdbc2/.lock
uv      16284 chilin  110u      REG               1,17         0 880687324 /Users/chilin/.cache/uv/sdists-v9/editable/49b881dd4e7c2155/.lock
uv      16284 chilin  111u      REG               1,17         0 880687326 /Users/chilin/.cache/uv/sdists-v9/editable/8f558e48644a548d/.lock
uv      16284 chilin  112u      REG               1,17         0 880687328 /Users/chilin/.cache/uv/sdists-v9/editable/65183deb247a8164/.lock
uv      16284 chilin  113u      REG               1,17         0 880687330 /Users/chilin/.cache/uv/sdists-v9/editable/fd07082a15f5bf1a/.lock
uv      16284 chilin  114u      REG               1,17         0 880687332 /Users/chilin/.cache/uv/sdists-v9/editable/0fd0f7861aa8ac41/.lock
uv      16284 chilin  115u      REG               1,17         0 880687334 /Users/chilin/.cache/uv/sdists-v9/editable/cc291d28fbaf17ca/.lock
uv      16284 chilin  116u      REG               1,17         0 880687336 /Users/chilin/.cache/uv/sdists-v9/editable/cbb324d0849b48aa/.lock
uv      16284 chilin  117u      REG               1,17         0 880687338 /Users/chilin/.cache/uv/sdists-v9/editable/910d92e3c8d47c11/.lock
uv      16284 chilin  118u      REG               1,17         0 880687340 /Users/chilin/.cache/uv/sdists-v9/editable/d4e32d44de491c27/.lock
uv      16284 chilin  119u      REG               1,17         0 880687342 /Users/chilin/.cache/uv/sdists-v9/editable/9d139bdfdb413ed9/.lock
uv      16284 chilin  120u      REG               1,17         0 880687344 /Users/chilin/.cache/uv/sdists-v9/editable/6d5069d05bbac4ee/.lock
uv      16284 chilin  121u      REG               1,17         0 880687346 /Users/chilin/.cache/uv/sdists-v9/editable/d20b00ac96d9f886/.lock
uv      16284 chilin  122u      REG               1,17         0 880687348 /Users/chilin/.cache/uv/sdists-v9/editable/ac93b333c09668fd/.lock
uv      16284 chilin  123u      REG               1,17         0 880687350 /Users/chilin/.cache/uv/sdists-v9/editable/e6c6518d577802c8/.lock
uv      16284 chilin  124u      REG               1,17         0 880687352 /Users/chilin/.cache/uv/sdists-v9/editable/a1e1aa0829d2c2ab/.lock
uv      16284 chilin  125u      REG               1,17         0 880687354 /Users/chilin/.cache/uv/sdists-v9/editable/008690e7bb679e0a/.lock
uv      16284 chilin  126u      REG               1,17         0 880687356 /Users/chilin/.cache/uv/sdists-v9/editable/37e753f38a532f87/.lock
uv      16284 chilin  127u      REG               1,17         0 880687358 /Users/chilin/.cache/uv/sdists-v9/editable/80a3fc931c073a38/.lock
uv      16284 chilin  128u      REG               1,17         0 880687360 /Users/chilin/.cache/uv/sdists-v9/editable/aef38b9ca6ebd4de/.lock
uv      16284 chilin  129u      REG               1,17         0 880687362 /Users/chilin/.cache/uv/sdists-v9/editable/a51f7101a302adc7/.lock
uv      16284 chilin  130u      REG               1,17         0 880687364 /Users/chilin/.cache/uv/sdists-v9/editable/c8651e576c93b622/.lock
uv      16284 chilin  131u      REG               1,17         0 880687366 /Users/chilin/.cache/uv/sdists-v9/editable/a74f78b17cb1507e/.lock
uv      16284 chilin  132u      REG               1,17         0 880687368 /Users/chilin/.cache/uv/sdists-v9/editable/33fa7891e7a1e07c/.lock
uv      16284 chilin  133u      REG               1,17         0 880624472 /Users/chilin/.cache/uv/sdists-v9/pypi/pyspark/4.1.1/.lock
uv      16284 chilin  134u      REG               1,17         0 880687371 /Users/chilin/.cache/uv/sdists-v9/pypi/gevent/25.9.1/.lock
uv      16284 chilin  135u      REG               1,17         0 880636620 /Users/chilin/.cache/uv/sdists-v9/pypi/aliyun-python-sdk-core/2.16.0/.lock
uv      16284 chilin  136u      REG               1,17         0 880624238 /Users/chilin/.cache/uv/sdists-v9/pypi/python-ldap/3.4.5/.lock
uv      16284 chilin  137u      REG               1,17         0 880624424 /Users/chilin/.cache/uv/sdists-v9/pypi/oss2/2.19.1/.lock
uv      16284 chilin  138u      REG               1,17         0 880624453 /Users/chilin/.cache/uv/sdists-v9/pypi/pydruid/0.6.9/.lock
uv      16284 chilin  139u      REG               1,17         0 880636624 /Users/chilin/.cache/uv/sdists-v9/pypi/crcmod/1.7/.lock
uv      16284 chilin  140u      REG               1,17         0 880638835 /Users/chilin/.cache/uv/sdists-v9/pypi/thrift/0.16.0/.lock
uv      16284 chilin  141u      REG               1,17         0 880624456 /Users/chilin/.cache/uv/sdists-v9/pypi/pyhive/0.7.0/.lock
uv      16284 chilin  142u      REG               1,17         0 880624232 /Users/chilin/.cache/uv/sdists-v9/pypi/atlasclient/1.0.0/.lock
uv      16284 chilin  143u      REG               1,17         0 880624459 /Users/chilin/.cache/uv/sdists-v9/pypi/hdfs/2.7.3/.lock
uv      16284 chilin  144u      REG               1,17         0 880624475 /Users/chilin/.cache/uv/sdists-v9/pypi/kylinpy/2.8.4/.lock
uv      16284 chilin  145u      REG               1,17         0 880638818 /Users/chilin/.cache/uv/sdists-v9/pypi/docopt/0.6.2/.lock
uv      16284 chilin  146u      REG               1,17         0 880624334 /Users/chilin/.cache/uv/sdists-v9/pypi/pykerberos/1.2.4/.lock
uv      16284 chilin  147u      REG               1,17         0 880687374 /Users/chilin/.cache/uv/sdists-v9/pypi/pure-sasl/0.6.2/.lock
uv      16284 chilin  148u      REG               1,17         0 880643302 /Users/chilin/.cache/uv/sdists-v9/pypi/alibabacloud-tea/0.4.3/.lock
uv      16284 chilin  149u      REG               1,17         0 880642181 /Users/chilin/.cache/uv/sdists-v9/pypi/multi-key-dict/2.0.3/.lock
uv      16284 chilin  150u      REG               1,17         0 880624372 /Users/chilin/.cache/uv/sdists-v9/pypi/pytest-timeouts/1.2.1/.lock
uv      16284 chilin  151u      REG               1,17         0 880635295 /Users/chilin/.cache/uv/sdists-v9/pypi/alibabacloud-gateway-spi/0.0.3/.lock
uv      16284 chilin  152u      REG               1,17         0 880644899 /Users/chilin/.cache/uv/sdists-v9/pypi/alibabacloud-credentials-api/1.0.0/.lock
uv      16284 chilin  153u   KQUEUE                                        count=2, state=0x8
uv      16284 chilin  154u   KQUEUE                                        count=2, state=0x8
uv      16284 chilin  155u      REG               1,17        56 880687381 /Users/chilin/.cache/uv/sdists-v9/editable/aef38b9ca6ebd4de/.tmpidnBpK
uv      16284 chilin  156u   KQUEUE                                        count=0, state=0x8
uv      16284 chilin  157u   KQUEUE                                        count=1, state=0x8
uv      16284 chilin  158u      REG               1,17        56 880687377 /Users/chilin/.cache/uv/sdists-v9/editable/33fa7891e7a1e07c/.tmpvLUJwA
uv      16284 chilin  159u   KQUEUE                                        count=0, state=0x8
uv      16284 chilin  160u   KQUEUE                                        count=0, state=0x8
uv      16284 chilin  161u   KQUEUE                                        count=0, state=0x8
uv      16284 chilin  162u   KQUEUE                                        count=0, state=0x8
uv      16284 chilin  163u      REG               1,17        46 880687379 /Users/chilin/.cache/uv/sdists-v9/editable/c8651e576c93b622/.tmpnCB3UB
uv      16284 chilin  164u      REG               1,17        56 880687378 /Users/chilin/.cache/uv/sdists-v9/editable/a74f78b17cb1507e/.tmpiFlLCK
uv      16284 chilin  165u      REG               1,17        56 880687380 /Users/chilin/.cache/uv/sdists-v9/editable/a51f7101a302adc7/.tmpeGcViy
uv      16284 chilin  166u      REG               1,17        56 880687425 /Users/chilin/.cache/uv/sdists-v9/editable/0f9b3789084f4140/.tmpU8ztzU
uv      16284 chilin  167u      REG               1,17        56 880687383 /Users/chilin/.cache/uv/sdists-v9/editable/37e753f38a532f87/.tmp9QtPn9
uv      16284 chilin  168u      REG               1,17        56 880687384 /Users/chilin/.cache/uv/sdists-v9/editable/008690e7bb679e0a/.tmp3L404e
uv      16284 chilin  169u      REG               1,17        56 880687382 /Users/chilin/.cache/uv/sdists-v9/editable/80a3fc931c073a38/.tmpDd6nPA
uv      16284 chilin  170u      REG               1,17        56 880687385 /Users/chilin/.cache/uv/sdists-v9/editable/a1e1aa0829d2c2ab/.tmpGVbwAN
uv      16284 chilin  171u      REG               1,17        56 880687386 /Users/chilin/.cache/uv/sdists-v9/editable/e6c6518d577802c8/.tmp3NjO5p
uv      16284 chilin  172u      REG               1,17        56 880687387 /Users/chilin/.cache/uv/sdists-v9/editable/ac93b333c09668fd/.tmpq2fR8Z
uv      16284 chilin  173u      REG               1,17        56 880687388 /Users/chilin/.cache/uv/sdists-v9/editable/d20b00ac96d9f886/.tmpsghu9r
uv      16284 chilin  174u      REG               1,17        56 880687389 /Users/chilin/.cache/uv/sdists-v9/editable/6d5069d05bbac4ee/.tmpay07SQ
uv      16284 chilin  175u      REG               1,17        56 880687390 /Users/chilin/.cache/uv/sdists-v9/editable/9d139bdfdb413ed9/.tmpplV0IA
uv      16284 chilin  176u      REG               1,17        56 880687391 /Users/chilin/.cache/uv/sdists-v9/editable/d4e32d44de491c27/.tmpxJRtbA
uv      16284 chilin  177u      REG               1,17        56 880687392 /Users/chilin/.cache/uv/sdists-v9/editable/910d92e3c8d47c11/.tmpDmKQ4Q
uv      16284 chilin  178u      REG               1,17        56 880687393 /Users/chilin/.cache/uv/sdists-v9/editable/cbb324d0849b48aa/.tmpK4Ocm3
uv      16284 chilin  179u      REG               1,17        56 880687394 /Users/chilin/.cache/uv/sdists-v9/editable/cc291d28fbaf17ca/.tmpFR3jeE
uv      16284 chilin  180u      REG               1,17        56 880687395 /Users/chilin/.cache/uv/sdists-v9/editable/0fd0f7861aa8ac41/.tmplONuAw
uv      16284 chilin  181u      REG               1,17        56 880687396 /Users/chilin/.cache/uv/sdists-v9/editable/fd07082a15f5bf1a/.tmprNowRt
uv      16284 chilin  182u      REG               1,17        56 880687397 /Users/chilin/.cache/uv/sdists-v9/editable/65183deb247a8164/.tmpNt8q1D
uv      16284 chilin  183u      REG               1,17        56 880687398 /Users/chilin/.cache/uv/sdists-v9/editable/8f558e48644a548d/.tmpRkGykO
uv      16284 chilin  184u      REG               1,17        56 880687399 /Users/chilin/.cache/uv/sdists-v9/editable/49b881dd4e7c2155/.tmpuVNiAp
uv      16284 chilin  185u      REG               1,17        56 880687400 /Users/chilin/.cache/uv/sdists-v9/editable/97ac4ac2c82bdbc2/.tmpibTe3o
uv      16284 chilin  186u      REG               1,17        56 880687401 /Users/chilin/.cache/uv/sdists-v9/editable/59b3fc416f552f8a/.tmp0UfXyJ
uv      16284 chilin  187u      REG               1,17        56 880687402 /Users/chilin/.cache/uv/sdists-v9/editable/81ed83f02afeccd6/.tmppLcRTl
uv      16284 chilin  188u      REG               1,17        56 880687403 /Users/chilin/.cache/uv/sdists-v9/editable/e23b45b1cc746bfc/.tmpMXRpb1
uv      16284 chilin  189u      REG               1,17        56 880687404 /Users/chilin/.cache/uv/sdists-v9/editable/d3d2d296c6d36f60/.tmpqXL0Ua
uv      16284 chilin  190u      REG               1,17        56 880687405 /Users/chilin/.cache/uv/sdists-v9/editable/99ba3ebb89790df6/.tmpXHFPVU
uv      16284 chilin  191u      REG               1,17        56 880687406 /Users/chilin/.cache/uv/sdists-v9/editable/45743f877212ec85/.tmpQNkhKV
uv      16284 chilin  192u      REG               1,17        56 880687407 /Users/chilin/.cache/uv/sdists-v9/editable/83f3be9a0f4ecc49/.tmpvZz6qo
uv      16284 chilin  193u      REG               1,17        56 880687408 /Users/chilin/.cache/uv/sdists-v9/editable/fed976fbf9c8a2df/.tmpcuOKl9
uv      16284 chilin  194u      REG               1,17        56 880687409 /Users/chilin/.cache/uv/sdists-v9/editable/16d841667ef486b4/.tmpuTxFME
uv      16284 chilin  195u      REG               1,17        56 880687410 /Users/chilin/.cache/uv/sdists-v9/editable/5c6b97221a82e4b6/.tmpSyD8sN
uv      16284 chilin  196u      REG               1,17        56 880687411 /Users/chilin/.cache/uv/sdists-v9/editable/9ec75ecd4fa64456/.tmpV30qqf
uv      16284 chilin  197u      REG               1,17        56 880687414 /Users/chilin/.cache/uv/sdists-v9/editable/e5204c77b346d1f5/.tmpqkXQ5L
uv      16284 chilin  198u      REG               1,17        56 880687412 /Users/chilin/.cache/uv/sdists-v9/editable/daffd7fb1938b330/.tmpgt0SLF
uv      16284 chilin  199u      REG               1,17        56 880687413 /Users/chilin/.cache/uv/sdists-v9/editable/82ad1b230e7233cf/.tmpQF4dtH
```

---

_Comment by @konstin on 2026-01-20 15:56_

That's interesting - I wonder if we should have closed those locks?

---

_Comment by @denyszhak on 2026-01-20 21:17_

@konstin I will check how Cargo does it but I would give this one a try

The locks themselves are looking correctly scoped - they protect the cache shard during the entire build to prevent concurrent writes. The issue is that too many locks are acquired simultaneously.

We acquire them early and each lock is held until the build completes but then we use semaphore that only allows `available_parallelism` (number of cpus). 

Should we limit concurrency earlier? Change prepare_stream to use `buffer_unordered(concurrent_builds)` instead of unbounded `FuturesUnordered`. This way only N editables start at a time, matching the build semaphore.

---

_Comment by @denyszhak on 2026-01-20 22:55_

@konstin Would you mind taking a look at the PR I've got for this one?

---
