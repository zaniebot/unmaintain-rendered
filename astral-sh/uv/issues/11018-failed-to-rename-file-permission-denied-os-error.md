```yaml
number: 11018
title: "failed to rename file [...] Permission denied (os error 13)"
type: issue
state: open
author: pierre-eliep-met
labels:
  - bug
  - windows
  - needs-mre
assignees: []
created_at: 2025-01-28T12:49:19Z
updated_at: 2025-11-05T15:19:16Z
url: https://github.com/astral-sh/uv/issues/11018
synced_at: 2026-01-10T03:23:53Z
```

# failed to rename file [...] Permission denied (os error 13)

---

_Issue opened by @pierre-eliep-met on 2025-01-28 12:49_

### Summary

Hello !

I am currently using `uv` on a private project (sorry for reproducibility...), and I often get the following error :
```python
failed to rename file from /home/pepmts/.cache/uv/.tmpZy5CEP to /home/pepmts/.cache/uv/archive-v0/_zo0z87nlFi76umWr4RI6: Permission denied (os error 13)
```
I get it when I try to launch `tox -e mypy` or `tox -e pylint`, and not with other linters, I think it is because these 2 are the only linters I have that try to install the package, here are the commands launched by `tox` (note that I use `tox-uv`) :

```python
.pkg: _optional_hooks projects/mts-api-python> python /home/pepmts/git_projects/mts-python-monorepo/projects/mts-api-python/venv/lib/python3.11/site-packages/pyproject_api/_backend.py True setuptools.build_meta
.pkg: get_requires_for_build_sdist projects/mts-api-python> python /home/pepmts/git_projects/mts-python-monorepo/projects/mts-api-python/venv/lib/python3.11/site-packages/pyproject_api/_backend.py True setuptools.build_meta
.pkg: build_sdist projects/mts-api-python> python /home/pepmts/git_projects/mts-python-monorepo/projects/mts-api-python/venv/lib/python3.11/site-packages/pyproject_api/_backend.py True setuptools.build_meta
mypy: install_package projects/mts-api-python> uv pip install --reinstall --no-deps mts-python-monorepo@/home/pepmts/git_projects/mts-python-monorepo/.tox/.tmp/package/13/mts_python_monorepo-0.0.0.tar.gz -c .github/tox-constraints.txt
Building mts-python-monorepo @ file:///home/pepmts/git_projects/mts-python-monorepo/.tox/.tmp/package/13/mts_python_monorepo-0.0.0.tar.gz
Building mts-python-monorepo @ file:///home/pepmts/git_projects/mts-python-monorepo/.tox/.tmp/package/13/mts_python_monorepo-0.0.0.tar.gz
Building mts-python-monorepo @ file:///home/pepmts/git_projects/mts-python-monorepo/.tox/.tmp/package/13/mts_python_monorepo-0.0.0.tar.gz
   Built mts-python-monorepo @ file:///home/pepmts/git_projects/mts-python-monorepo/.tox/.tmp/package/13/mts_python_monorepo-0.0.0.tar.gz
⠦ Preparing packages... (0/1)                                                                                                                                        
⠧ Preparing packages... (0/1)                                                                                                                                        
⠇ Preparing packages... (0/1)                                                                                                                                        
⠋ Preparing packages... (0/1)                                                                                                                                        
⠙ Preparing packages... (0/1)                                                                                                                                        
⠹ Preparing packages... (0/1)                                                                                                                                        
  × Failed to build `mts-python-monorepo @ file:///home/pepmts/git_projects/mts-python-monorepo/.tox/.tmp/package/13/mts_python_monorepo-0.0.0.tar.gz`
  ├─▶ Failed to write to the distribution cache
  ╰─▶ failed to rename file from /home/pepmts/.cache/uv/.tmpZy5CEP to /home/pepmts/.cache/uv/archive-v0/_zo0z87nlFi76umWr4RI6: Permission denied (os error 13)
mypy: exit 1 (12.73 seconds) /home/pepmts/git_projects/mts-python-monorepo> uv pip install --reinstall --no-deps mts-python-monorepo@/home/pepmts/git_projects/mts-python-monorepo/.tox/.tmp/package/13/mts_python_monorepo-0.0.0.tar.gz -c .github/tox-constraints.txt pid=16467
  mypy: FAIL code 1 (16.30 seconds)
  evaluation failed :( (16.45 seconds)
```

After a `uv cache prune`, I relaunched the same command and it worked fine. I had several cases like this before on WSL/debian, which is why I moved to WSL/ubuntu24, but it does not seem fixed, even if I have it much less often than I used to.

I hope I gave enough details, happy to provide anything else needed

### Platform

Windows 11, WSL 2, Ubuntu 24 - Linux 4.4.0-26100-Microsoft x86_64 GNU/Linux

### Version

uv 0.5.14

### Python version

Python 3.11.5

---

_Label `bug` added by @pierre-eliep-met on 2025-01-28 12:49_

---

_Comment by @konstin on 2025-01-28 13:01_

Just to check, do you ever run any other those commands from different users, e.g. with `sudo` and then with a normal user? When this happens, can you check `ls -lah /home/pepmts/.cache/uv/archive-v0/_zo0z87nlFi76umWr4RI6` (with the path from the error message)?

---

_Comment by @pierre-eliep-met on 2025-01-28 13:09_

Hello thanks for the reply !
I always use the same user, on WSL, there is only one user in my installation anyway.

I recently got the problem again :
```bash
  × Failed to build `mts-python-monorepo @ file:///home/pepmts/git_projects/mts-python-monorepo/.tox/.tmp/package/17/mts_python_monorepo-0.0.0.tar.gz`
  ├─▶ Failed to write to the distribution cache
  ╰─▶ failed to rename file from /home/pepmts/.cache/uv/.tmpjXWCEU to /home/pepmts/.cache/uv/archive-v0/CF5kJ6v1az6ACa_gXuH4m: Permission denied (os error 13)
pylint: exit 1 (7.93 seconds) /home/pepmts/git_projects/mts-python-monorepo> uv pip install --reinstall --no-deps mts-python-monorepo@/home/pepmts/git_projects/mts-python-monorepo/.tox/.tmp/package/17/mts_python_monorepo-0.0.0.tar.gz -c .github/tox-constraints.txt pid=20909
```
So here is what you ask :
```bash
❯ ls -lah /home/pepmts/.cache/uv/.tmpjXWCEU                                                                                                                       
Found existing alias for "ls -lah". You should use: "lsa"
total 0
drwxr-xr-x 1 pepmts pepmts 512 Jan 28 13:49 .
drwxr-xr-x 1 pepmts pepmts 512 Jan 28 13:49 ..
drwxr-xr-x 1 pepmts pepmts 512 Jan 28 13:49 mts_python_monorepo-0.0.0.dist-info
drwxr-xr-x 1 pepmts pepmts 512 Jan 28 13:49 projects
drwxr-xr-x 1 pepmts pepmts 512 Jan 28 13:49 spelling

❯ ls -lah /home/pepmts/.cache/uv/archive-v0/CF5kJ6v1az6ACa_gXuH4m  
Found existing alias for "ls -lah". You should use: "lsa"
ls: cannot access '/home/pepmts/.cache/uv/archive-v0/CF5kJ6v1az6ACa_gXuH4m': No such file or directory
```
I guess the second file does not exist because `uv` could not create it yet

---

_Comment by @konstin on 2025-01-28 13:11_

Thanks! Could you also check `ls -lah /home/pepmts/.cache/uv/archive-v0/`?

---

_Comment by @pierre-eliep-met on 2025-01-28 13:13_

There are really a lot of them, sorry for that
```bash
❯ ls -lah /home/pepmts/.cache/uv/archive-v0/                                                                                                                     
Found existing alias for "ls -lah". You should use: "lsa"
total 0
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 -i_RyQdPIPClihCqx9f98
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 -ulDj5oFpZZ1lBVgqXTM_
drwxr-xr-x 1 pepmts pepmts 512 Jan 28 13:47 .
drwxr-xr-x 1 pepmts pepmts 512 Jan 28 13:49 ..
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 0JGqWYO75Rmt5vlpj1Phf
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:51 0Pwr1uFP9on913YyTI_eg
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 0iEaVx77eE4D42JsFxD_x
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 14DRI4tPHd-HbCmWisUtB
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 191ZHEtX0kQb3axsIGGuV
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:49 2LMoIuiNW4Uq36WjUHeLG
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 2qa0ECVNjUGS1L25ypVRO
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 3FJatTYTIDrrWQNRNqSCl
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:57 4-3E0CnbxjvciqPka8-I2
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 4J0-ht6N8K44G4DSdDRu2
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 4Oyi5a-spoeTqpfrfPBkQ
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 5tXb_jACZRue-Di7AWQEJ
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 60ChRVw21atOkdVMIB2RZ
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 6DsNzyHioUp6J0VVYbo73
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:51 6JV2X8PTkOgb5m16ek0p9
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:57 6_AL-uBsqCnZlwYV-w0ba
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 6cOF4FyzAPOTX8ncw7r3l
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 6scqwSKl9fN7JkPEXpTyO
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 7FgYmci1Y32r2_9qtPa0g
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 7d4Jw0igOJFwiqGabYq3N
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 87cujP00c1fRME2VzYKcT
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 8E6HTikEINDh5baaUsZt9
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 8IUxT_JC78aSgOq2-QIcp
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 8Z-wqFsJot_1ppADuGgV9
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 8q40p-dD7SmsYr1Ie3j9V
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 8yHtDGaAksk3hM2jdL4dH
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 9UTK3ETnqylkZgsxQmRic
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:49 9ff2mqkVBW4PglKAaSypc
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 A6KJ5Unwg0bt84kxBKMft
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 A9Y7zS_hFd1N4lsQir_TS
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:54 ALVi-ltdwAcQRRlhBkUvS
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 AYSq4MYM1sOSjuOByI_oZ
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 AYnePJPaQatBg7xMH_F1f
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 BG-kUlujcwV9Vjq1IaEUe
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 B_DMW8dV3Dk_-uJHu6piE
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 CGuDydeGTc1TBpl2-WMVg
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 CNqR0p4-SUpAsWyLmIaz0
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 CVksV88z2lBaNFihvmy40
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 CdIg6JjTW4Q4Af5qoGT8L
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Chxq_IB6n8BHHBWZv5IPH
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Dd-WMjajYftKcKFfmYhBj
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 DhiIN1FlIxf6ynMqwLI4g
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Dqj8-Kz-COmsorJEqIlMf
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 DtXWWc7rgnIf92OIIbAq8
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 EAoJwTmzNvB4TMgsVJvn8
drwxr-xr-x 1 pepmts pepmts 512 Jan 28 11:28 EF2uYQfzKeFvVSd_vt7aB
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 EHiJ7vOh428UkBJwlX9x5
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:49 ENCZeRf0y4ritKhvM_dtX
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 FWp2RFafk3fl2D9qp-kb_
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Fe5VbCauXlVTZhEZHzO5J
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 G_6QxxNMCEF_0yse_d0nu
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 GcJsAefSXBLTyYSGvBnRm
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Ge2M4ReAlsS5P8nDivU0-
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 H4Jfz4_9rBVd2j1ZIcW19
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 HMq5P2nApDXJPBUPbGxpD
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 HNPj3JFjsMWskfwO8_GBc
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 HfSPX_ZQvbXVsu-nM2Kwx
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 I1I3TOBQTeKUgZe3Cnzpy
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 IV5nBJ8AcG3rv620aAa29
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 IXwnGW8offYkO3ooUnQKL
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Ifu4PXleNGEfl5L8tkk1y
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 IkFBiEFCSYmirHA32IIO6
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 IuJ0HxS4n7Z-_hezPSkGB
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 JRR6fXB03G8-oqae4bq01
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 LFqto0mzYPqGTwFzAnZIw
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:54 LaQL6UwNnyOBw4r6ozUR9
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 LncRyrJ2plUey-imeE5Lw
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:49 MPJVCqxkk4UauvL77CKn0
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 MdbeG4O_7Aayb8x6uw29_
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:51 NS0hPJ-c7aScipaHuUP8a
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 NSo6Cbpn4pKyzfwv9jWwF
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Na0EQTL2_TkFZQDn-OPch
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Nt7TnM1pUXIyq_b4J572A
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 OpzpDcLmvTaldRlYDpuHO
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Ozv_HT2_vEodQBnBkeay_
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 P5vzLO_Q-h_NpFuwUpLPa
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 PwjJ-fGXB5HAMFiKK-Ofm
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 QBgK9f5zIgH5rk59GcW8V
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Q_mBLkVI-ivjwkHgK3KWH
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 QfhihnPbjBWdTsSgAyBqg
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 QqO69NWNI3AVRaalU9mZi
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 R2VnqgBByfB4wAhwCAswO
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 R9pLu9meJVHXekx0Ktubf
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:49 ROsGjVCQ8sDhmCXczXaYY
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 RwzSfd8lefXtpCK_WRMQv
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 S-aPzlMqbhZ7HlEJrYPrG
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 SCt2Ae_C_1x64wp6OqUay
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 SXDf6Y-T3sw5aYQ8rnEE6
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Stim0MXOg4Fv31WGE2s8o
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 TDtcLgpUjnBCwSQ7yN9n1
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Tvmhm6wZc2Nv6OzqHdMO_
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:51 UEal4_7FVcC08P2-77X88
drwxr-xr-x 1 pepmts pepmts 512 Jan 28 11:26 UNSTfcZxp9d_nMQmXuZgt
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 UbjY5if-_ME4tb_Te1XZd
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 VAUuODYy-OvXkCOkSXtrV
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Vio-0sTry7tvC9GqZRPws
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 WS3qzQmiFAn1yXq8hOYtn
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 WVxKa6tekMBPXmNQOQ6Ov
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 XRCKQQj7u1WAEStpKzZaO
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 XWrgE7FbqvWKin10pwwBZ
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 XjiG1iuPPTaeUzyfSgBIS
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Xu0pe-htXkz-HAgop0liJ
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 YCOwYEA7g89IhH_VzsbSu
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 YMq99ide-HrFLBfGGWSjo
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 YQCAMCVSRPBK-wGNQDqTg
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 YZx6oXQh4Zkg-hGOGAz1L
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 Yc4Ct8e2qyP6kCWBmtL-Z
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 YlZBZMd_KiSAkGZI0RVOv
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:49 Yt7AYeV4mXYBAF0a54wVN
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:50 Z85jf78Tea4aerT6QtFsX
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 ZkdFmmkObS4Gp4mQfZmdN
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 ZyzKn5lA5zcIh02DD7zty
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 _HS-Fh1oCvHLHPwb-RGE0
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 _URWQ1aFW7x4GMhr5LvTM
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 _fE5i49qcd15RgNWhNOxs
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 _gbsm7Tc-NTRb0ch1IRvJ
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 _lx48Fm2yYUdDmbZnfOBX
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 a8p_OlzYvZjo927Sdux8U
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 aVggZBeMMaONgHououHa9
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 asF3QWVCPEA7sNLkNeSY0
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 b0ifvpf9oF9LPpb-khptC
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:51 bVynPdobmsXT9_cSNvLPk
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 bWu6tyxWUtDLmtm2uWlGk
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 bXqRi_H8x7oS3r6AlznyE
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 bkfwMsSiRJAYj2G85WDz6
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:50 cD9fDX88Fkd0TwT9s0UaS
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 cbD9ExFvvsjHBq_8kthkJ
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 cfr3evhh5ntWW3hhZmoyH
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:54 d7swaIpiWbL4UPl1HHmBz
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 dBWd3kdo0sUiWH8oywgNX
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 dUSPqnw1BAHpLrzqH8Pg5
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 dk0zwP61kFfgKHB7SOl7q
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:50 eU0T8jE01oVTwnFm1a95j
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 fw4X6LzAHwwAwfZLXe_k3
drwxr-xr-x 1 pepmts pepmts 512 Jan 28 13:47 g0gWc1jUa9hRSqafU3BT-
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 g0n3r_Y8PgkjbUvAzCkYx
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 gf6Ib0YLJFSEpFlbTNloJ
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 gl9qOLJ8l3ffo_b0Z_xnX
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 hWmZfvJdI5568j2mgXtIY
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:49 haxkOdnFUCOXHCo0mEfo6
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:50 i3iVyf-BDgB7Cq-gG9gRa
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 iC1xlh5G2LM0S-lQT2ogA
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 iDbnrlmZ5i84bgX0crIv4
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 iIC5gZdkXRNV_h-_eXccq
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 iXExJu4OWxCfYaqnWfbtC
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:49 ijV6LgGm3RlOOfuq-wu0L
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 iuBk2VSoTA7J8IJYbwDtt
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 jWP6LF0bPpsuy_IJPMoq7
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 jcBu7boSoSgtfnGeqEMSq
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 jru9CyV4dZ1yUlhrLDuE8
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 kA9JLPCUwSrwKSSpVI12W
drwxr-xr-x 1 pepmts pepmts 512 Jan 28 11:34 kC3BbukZo-kIiOvf9voeu
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 kDdYBOskuHe5VGVPOJOrX
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 kEd6Vui0cf7_Md0DRu3om
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:50 kHjUOja7lAnkz3V1ntPga
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:50 lK1rTsw8yRlI2XSPQidg6
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 llTlfG0A3qAY1CPG3vT-M
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 mU_dOsaduBcRd8YML_S5V
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 n4z4gk2MVyAxKNDJiB1_T
drwxr-xr-x 1 pepmts pepmts 512 Jan 28 13:46 nBpkkhpkNtHmUnBli4Osi
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 nLBdHIW4aArAt4Z9y6eZA
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 nTvZU04qkK9Ryilicu6gD
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 nX9GEpGMLe2DtPpJqGQfh
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 nahvR3dW0d-75c8gJ3KWW
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 niiKjBqP3kIo-uyrqUFYc
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 nlzoGpJf4bsb26M4c873M
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 nzBm39GyHweAObhwFjWtw
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 oIwKugTajIIOXQU2IC5Mk
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:52 oNe1GBhtyXF_DF-JqPwiP
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 oUXcSRnJ8tPuiOt-4mP6u
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 oYX7yE6AswLU-wSR1hl_I
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:49 orNFakaPfy9HjhxscZTkD
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 oy2kaEIp3NyaZLD7j4klH
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 p7255-iaZCXrwLU-jIY1K
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 pA9nfrpAb5L0GW_EKGb6M
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 pEAzjXTxCa7GfNPbVtc9H
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 ptat9ICd0PeNfxjjQhpk7
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 puMsec_Zk0oY6hSF5VsTu
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 qDENrLKnYu8IpiUUbKuBx
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 qRzTic0dupSm7k7XtKG_m
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 r_kKHxmPRZMw-tGM6W3IH
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 ryqXiEEvqFLvIWUiQSGVT
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 sQOrQDOqNQaSI9y7BwW9M
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 si3WQPIv06fWRht042CRw
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 t1yvOpWEiYNQ8-FsolNt-
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:49 t2GeS0HVXYssrw_nwM4Ju
drwxr-xr-x 1 pepmts pepmts 512 Jan 28 11:43 tUV54jWSId2T_et6fdnkl
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 tUni6BlHDeZ61BQX1woqi
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 uetfZelhF-zjJQ2Q7ahru
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 vE9UOQEDhm2B5a_g-CLk6
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 vvv6suldTsITSr6EtGc5N
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 x380nUT3Se6FoPR7hd63-
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 xtbZtaZGbukVmsNVCrvr8
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 y_tbQPqcMs07RHdNJtv3_
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 ydN2Eb5JS-YpEKWmYtMos
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:49 yrVFK3Yyb4bF0EsmTqbS7
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 z6wfArLF-YSdvpnMFPKoF
drwxr-xr-x 1 pepmts pepmts 512 Jan 27 17:48 zeAyPNc-E7yCnVVuVVLPs
```

---

_Comment by @BenGale93 on 2025-02-07 09:22_

I also get this issue but find rerunning the command fixes it. Sorry to not be more helpful but it is very difficult to reproduce.

---

_Comment by @przenz on 2025-03-17 18:03_

Still occurs on v0.6.6, rerunning allows to finish in the end, but with a lot of dependencies it has to be rerun multiple times. I'm able to reproduce every time with cache cleared. Running on WSL1/RHEL8.10 on Win10.

---

_Comment by @xxxpsyduck on 2025-04-01 04:54_

> Still occurs on v0.6.6, rerunning allows to finish in the end, but with a lot of dependencies it has to be rerun multiple times. I'm able to reproduce every time with cache cleared. Running on WSL1/RHEL8.10 on Win10.

I confirm this could be reproduced on WSL 1, uv 0.6.11

---

_Comment by @RichardDally on 2025-08-07 10:00_

Also having this issue with Python 3.11 on Windows with uv 0.8.5, I've tried to specify another folder for cache. Anyone in Astral looking at this ?

---

_Label `needs-mre` added by @konstin on 2025-08-07 11:03_

---

_Label `windows` added by @konstin on 2025-08-07 11:03_

---

_Comment by @konstin on 2025-08-07 11:07_

I can't reproduce this unfortunately, and I don't see any specific patterns from the reports so far. We need a way to reproduce this or find what sets the reported cases from the working WSL cases to fix this problem.

---

_Comment by @eademir on 2025-10-18 10:53_

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh                                               
downloading uv 0.9.3 aarch64-apple-darwin
no checksums to verify
installing to /Users/eademir/.local/bin
mv: rename /var/folders/yv/68x9rg717xj4zlwgrtbrr_240000gn/T/tmp.HVx17VjrS6/uv to /Users/eademir/.local/bin/uv: Permission denied
ERROR: command failed: mv /var/folders/yv/68x9rg717xj4zlwgrtbrr_240000gn/T/tmp.HVx17VjrS6/uv /Users/eademir/.local/bin
```

I had this problem while installing uv. I had to move uv binary manually using sudo and ran `echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc` command

---

_Comment by @konstin on 2025-10-19 08:58_

@eademir This looks like a different problem, could it be that `/Users/eademir/.local/bin/` or a parent directory is not owned by the current user?

---

_Comment by @eademir on 2025-10-21 13:51_

@konstin yes, it is owned by root. `drwxr-xr-x 4 root staff 128 Oct 18 13:45 /Users/eademir/.local/bin/` I wanted to write it in here if someone has the same issue and finds it here.

---

_Comment by @konstin on 2025-10-21 14:08_

I recommend changing the owner of `~/.local` to the current user instead of running uv as root, this also avoids similar problems with other programs that write to `/Users/eademir/.local/bin/`

---

_Comment by @mokeyish on 2025-10-27 02:19_

How to solve this problem? I don't want to delete .cache/uv anymore, because it will take a long time to download.

---

_Comment by @konstin on 2025-10-27 09:37_

@mokeyish We need instructions how to reproduce this problem on a different machine, otherwise we unfortunately can't give you a fix.

---

_Comment by @xxxpsyduck on 2025-10-28 04:55_

@konstin The WSL2 works well so you may just add to the documentation that WSL1 is not supported

---

_Comment by @mrdaemon on 2025-10-28 05:48_

I used to have this issue on WSL1 alongside `Cannot allocate memory (os error 12)` during `uv sync` of a project with a reasonable amount of transient deps, and the configuration that ultimately works for my setup appears to be:

1) I have windows defender exceptions setup for the WSL1 distro directories
2) I have `export UV_CONCURRENT_INSTALLS=1` in my shell profile

I'm on uv 0.9.5, project is using managed python 3.13 (not that this last part would matter).

Either of these things not being done results in either os error 13, or os error 12 at intervals.
Sorry for the super anecdotal, voodoo information

I previously had `UV_LINK_MODE=copy` due to suspicions that the weird pseudofs for `/home` in wsl1 was not having a great time with the linking, but turns out it makes no difference other than tripling the time it takes to sync due to super slow copies on that filesystem.

Sorry for the super anecdotal stuff, I wish I had more meat to provide, but hopefully it will help someone, somehow, maybe.

---

_Comment by @pierre-eliep-met on 2025-10-28 16:08_

I cannot really test anymore as I don’t do python development for now, but I had this problem on WSL 2...

---

_Comment by @99hats on 2025-11-01 16:12_

I had this issue while using Codex. The cache dir fell outside it's sandbox and it needed to elevate it's permission for uv to access the cache dir.

---

_Comment by @camper42 on 2025-11-05 12:26_

I added `$HOME/.cache` to Codex `sandbox_workspace_write.writable_roots`, but I still can’t use `uv` with `codex` due to:

`failed to open file "$HOME/.cache/uv/sdists-v9/.git": Operation not permitted (os error 1)`

This seems related to how `.git` is handled.

> On macOS (and soon Linux), any writable root (including the cwd) that has a `.git/` folder as an immediate child will mount that `.git/` folder read-only, while the rest of the repository remains writable. As a result, commands like `git commit` will fail by default (since they write to `.git/`) unless Codex requests permission.

Is it possible for `uv` to avoid writing to or depending on `.git` in this case? Many users are running into the same issue: https://github.com/openai/codex/issues/1457


---

_Comment by @konstin on 2025-11-05 14:01_

This is something that should be fixed in Codex, uv needs to do git operations in the cache to support git dependencies. It's odd to me that codex would make such a choice, banning a common and reasonable operation. Codex should definitely not commit there (and really, not modify the cache at all, which only uv can do correctly), but it should not prevent other applications from using their cache.

---

_Comment by @camper42 on 2025-11-05 15:19_

Thanks a lot for the explanation — that makes sense. 

The issue occurred when Codex executed uv commands inside macOS sandbox. Codex didn’t attempt to modify the cache directory directly, so it’s likely a problem with how Codex handles the sandbox environment.

---
