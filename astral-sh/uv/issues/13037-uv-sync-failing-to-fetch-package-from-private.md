---
number: 13037
title: "`uv sync` failing to fetch package from private index with 302 error on 0.6.15"
type: issue
state: closed
author: mfriedy
labels:
  - bug
assignees: []
created_at: 2025-04-22T02:51:46Z
updated_at: 2025-06-16T12:46:19Z
url: https://github.com/astral-sh/uv/issues/13037
synced_at: 2026-01-10T01:25:28Z
---

# `uv sync` failing to fetch package from private index with 302 error on 0.6.15

---

_Issue opened by @mfriedy on 2025-04-22 02:51_

### Summary

Hi,

With the newly released version, my `uv sync --frozen` commands in a docker script (referencing :latest) have just started to fail.

Issue seems downloading wheels from a private GCP repository.

```
  × Failed to download `xxxx`
  ├─▶ Failed to fetch:
  │   `https://europe-west3-python.pkg.dev/xxxxx.whl`
  ╰─▶ Invalid 302 location URL
```

Everything works fine when changing the docker image to 0.6.14

Key parts of the project.toml

```
[tool.uv]
dev-dependencies = ["pre-commit", "ruff", "fakeredis", "ipykernel"]
keyring-provider = "subprocess"

[tool.uv.sources]
"xxx" = { index = "yyyy" }

[[tool.uv.index]]
name = "yyyy"
url = "https://oauth2accesstoken@europe-west3-python.pkg.dev/zzzz/simple/"

```

### Platform

Docker build in linux

### Version

0.6.15

### Python version

_No response_

---

_Label `bug` added by @mfriedy on 2025-04-22 02:51_

---

_Referenced in [astral-sh/uv#13038](../../astral-sh/uv/pulls/13038.md) on 2025-04-22 03:03_

---

_Comment by @zanieb on 2025-04-22 03:04_

Thanks for the report!

Would you be able to test a development build to get some more information? If you go to https://github.com/astral-sh/uv/actions/runs/14585787652?pr=13038 — at the bottom there are built binaries. I've added some more information to this error.

---

_Referenced in [astral-sh/uv#13040](../../astral-sh/uv/pulls/13040.md) on 2025-04-22 03:27_

---

_Referenced in [astral-sh/uv#13041](../../astral-sh/uv/pulls/13041.md) on 2025-04-22 03:32_

---

_Comment by @mfriedy on 2025-04-22 03:32_

Here you go!

```
  ╰─▶ Invalid HTTP 307 Temporary Redirect 'Location' value
      `/artifacts-downloads/namespaces/xxxx/repositories/yyyy/downloads/AI3WYFYeGq-wGSMwm7U4cqhe-2XcYgwW3Pej8pKtFn8ILFkPea_1G4v_5npA3Rb3L1EVl-6DMuf9rBncsNhD1-Nk-QshvLe9ziohnyppg2yb_PU2J_EfwiWZ259Zub8sqLaKFZhMsAezuToMEA69u2vCSizJ2_9DP-kXN3ziiFieUTXAUvPfzQio9syUp7Y2o9qbVMgvcmIP4IhLfoP1ImznBLg0eJ0A7QUC8ASGZponB21twkO2qNCJHJ_R3GpG0_tUlTSoeYU7S5gjyFtRXuNz1ZY-oJp2CUSTa3gL4BNbNK1JrvIzZGebEengEEAC30QKcVWEB6w0aFjWegIuperiMfloho5kcLa5QQe4GQXXLktE9fxo4l_1klcCzJleP9nZDrXGscDseOVZ3cDeiw4xAF0MNih1S_yZEQ94nqNs1_29xPQ5Ln5jopM_tmtAzhlRkdLJLciyMp5L5RHq_sWIa4Foq5srb_5QY_Kihr6dfnDVIkayqe2vl59kbDeRuQe33cF4usnimvh_jNS0fqfRQvi3BOu9jLVuvK59K8o3lSLqIqPIdUAgvjPdOG36YVNW01dr2A74h6a6tGale8_RsAHFU9flJG3NX13FkHm81oUGyciExIG30yaOuS4v8GQYt-HZmSH3jk2bW40a8XqarPsOAGLyobjejljUOqsEQ8KEsw5L-jOscj8wdE2FA29v_IYXtTlfzjiAO3LY0D2CEzUIlpFG4RrCEQ2N47CvmRqpdFz9saPs3aLHIl-I3neiArs1yE4hHJsPBFQmQ0lfnjt9hJBJ3rmC9TgN-NbI4v_tE3W5atw_No5yYj5Kf5GS_ct_ANe43ss_efipzd2YY3rCg3YIQjeOo-POSUMSk6RoGw-3mATT4m9nuuhQ8rQpBncLF1aiedpy7FpYHJo9e-Y3-lh12QvSE8Uy7pQanRKmm2vd6irgfFO3ZOVX3cj307ANvA5z29ojaGvfAm8fiJRSjT2pEJ2LJgeaGMpPQW0j0qJqaSk99m6zSO1nwWSuvXBq0zd9RJwbwlBoFWoloPDI6-o05XHL8UBIeTBoUTU09OfpyaUknVKG4F3bju3WwD8Ijh6V8tnT_KibJLQBqkc5AcK7mT5D7wmrNV4Nb0R8LhpgigIvxcHfX4SfG-fqEC4becmMEpebXT_nDK4S4RBlS1ZUhu5bOoCtMecLDtcco-sI-XvtxLvHa-Nd7AfNpERsCjEEsuBIwT_q-E0ZULzs8nGKvtoiFnDDnEs6KzH-HktyBYezaEUSwdibAoh2BRBfR48TxPpwN3n0IMGSmqJP6x_dusOrPRAWnGEGXxqsg66FA_TU2-d2E6bm58QuY6fYKQewJMISpHuGinwXnPGunR6y-FhRtYngmbJyaqRbOmnrsuInwpzUmPUqx008GmKpSYCK0zHtuHD9fWRSrRxpdVo288mYNz_5OLNAKJpj9M7c68tCn_WTbVWZ6v9gJEguzqKJjArzGwRsVM5OoOoqCunPUTkL5aggvfseZeoV4DzF2WZPkz0TN2lrwn07w1pf_Hr_ZyahjKhjvX-BfmQVcvTn6kvml9aOUSFLYUO4O3TxvDt1jBzZj8EwhXVUqaPvpybxHk0oYKNL_vnndE_FjmAJ__SjQ2MHbFopO-zVbYeGmPM_qqW_ZOnxykYJUuY87toDvq94UE8EZ-HvESbc8qRZ89pPNIbjR8jDO0MDfGnqj_6t8A3hx3Cdlg-EM2k7o7mJPPP8QuYY5P3Cw50cfDs0AS6zTI7_pNiXXcArQdwOGl8daQ1ftvFazc_O_9kkvKrhNon1kwezGmZCW8oUrtKqDoayAc3YITwmieoFXLYvTbyDW1SX2jtdGyOAFfinRs4wmhntpRoxWTzM4AJNgb0iUup1l74EZe7l7ocsTisnOTv3crRy83H7Ywu7pyL2kiHmP_WwO8_FkeEw5jLXZD4lIAs_6aVFlfDrs8UftFL6lr4JirXg5tU8Yo4hxRaZXODF1q1IXEPY03pNRJm6i_1Omf-h47anCPUYQZ_cO691GREBILsmnC9rBz_kpIQFFxEOiizorOq5jzNP9J0bthjusz6cY3Uq-1wsDtK048VcJGf5s5mRrYgZisZ91Ggx_xbLlPQCW9u0TmJu41dqjimKNcx83_Q1OJaiUm_S3TipYsYiQeuwNHVAPAyEv7QNWnsiaRSsUnPu-YqFs9T2ktSzwxgLiiExKHqDIIs6zfXt13Q_hfTQhuHilXGl17dxL2Ari_hjeGWoGwVtnz1_wzZ3U9Swt9moNumxaZLITyHge8AY-Y_FRpprHiHZk7b5AGwAlFgdrIpZKnvAJedI--I855wOqRtlq8K1jFRxwWVXvhdUJ8dkiYQFJkr4a1pFOhXMsFoUkITogGjvJby1vSHlIuFxf-GxUndpdUX9g30PEXbw7xdKMZ3r5_HB1R1gIvW1ghFqjKL8X1Om3HxLUq7X_3zR75PpplhGPz81nJVRT6C1H1nT-vVjtjmAK9xA2LQ76065Ol30ZUE75BDu3BmHmHYfmoYBfWhAD3IV9SII42KhNxFoEhmuE-HJ8lU7qHOa9ipyFYCpMML4dPoxedn4cQlqxNFCGpOkyWPZx1HwIV6Uy3OdoaelsMZ7cfDW6-O46QPyrEAn_oOy2rIgJKl0T-QQjDgv9je77ttBZQygIM1GiO-APB3jtZXKWvwegp9tWH9kZXd5bwDpM4LvbMmjckPfnGihkfroa9XvM-eEyf50zmJWgsl-Mo8EdE5siZeI8CJfARjhqD2I0kdZCkzvrQg-UxaoSruGzGZA-QV4pARl936valroBBx_MSzwUoj6us_ym9Cbj07eJwethBvn48TlCIDOjv0looXl6QGVLeY37sPJ0S-h0jyhTD-zLuvG8KItANxc_LlMZQ9bXQMsh5_kSloixv9zdYODNce6CJ0WW_BzG9--MHyqBK7ohZ-BydfSrx7q6R3tP9QvdHtz1QU7DfsKttZ0jAIJqH4RGBvGX9ZMIUVdEWW0Svzwg55Rsxt6RaUpLVQmB9P7Rc4gaxsI0YSnrRk_vyBmgdnexfj_TmFAr06ooXPK-pGRae__9d93K8fwZWEn_BevSeTTVf3GvZS3cE92e1gEUuBHzT8Hw-Ti0nmFh2T-JbQUzuH5D4VuIYkyQtY9bN2T-1eloIg6C9PsC0ReqlZE101TDdLWzyMGGLtFTd1zNZj8NwJLhVwPlK1S7cvOdd7jHXn3htHW0MoRwCzUSI34VjzJjRM_cn1OkW2aP_EP33NqU1eBEXvE5Bzl9q5murv1qq-c3XR0ScZwKKO4QzuqpMmSDlVIcvsne2jtmVNC5c9gR-2AIyKGf-g5RTX-27mqxbB9G58sGbu0BJRXr9PwY3Wboy-lWOmR1tq-9C459Bfkp_YGsC9GPrNMm00Y0mK79aFFylrROtElSm8WsYD8U6BV--tibn1JMmoJ2RtdrLiBKNX_kvBfMBAQUdanRFqhZSpsGQpU2-YcIPOgh0EZzWlbwRl0YF5FzLSQGofKZhEwukI0b5WRMRsj6SULVTF7VyafuNDtqYgFrWNA2c0HJVtqPrMzvuDSOVw7EPJhEFC4xHxLxdRO_KXJr-f3af16bsaMHp83UeUBMNLZi2UDP5IgHHVS4WjC5sno6gGqmMzvfhuBLhYH88xl0vYQLvxcm__rc_GU8f_A4lrs65ArrpWOwIksrLLkGqOpnod6g3z-S_2LdBly3fFRRKOVU6NBd7aTGKXGwMgGByLbusuKMLGT3eFAms5SiPpRfldRRZMSUT-xNWM2f8gVWRO4cWc20l1Ahy62mc2WzcK1lMnQC0DFnwbOaSWm8BpbVJiNKyxTJ0UqutQ5HNLoYBLh72K9gdUmYBABiV4FBZQ-pJKB22TjdtJyFRok-smq9t9xOSSjtJ3Sdlq0EFNUI5zfydLLK_w2Y81uj6ZLca7aSFQ7XXAFczQ129HEc4NBx4ovpkEYUPBKKiQoUyP-HDg0nO16oTm6lwNYg8LE8-6PBCGt0-i2WtUqjP9nPlRNLkrT0WUFKxnNnGrpNF97O9rn0bveUaPF2N1XuZ_noH6kz0Tj-DjmBaMb7GqrRBW6kZvdwV4epY6s7m_2F77jl9UDdvD1zrdPHy67EDw681yyhQxuLcNwunhM4P1IJ0fu6rIFw-YTJTJJIJHU3Vr95a26DBGffr2JvDq1XpGXrSsNKktUSPgtUp0j0smN2HCcQn6_dcnLRuwD-T0SDC0FOl9z01fC4K-nAISCvHHMZEb9AmQfKwn7J8lMD5N1NfGr3dkJHETu75N7zbkPtp59Mat_Z-_mSSjKcat0lsPq7O_EFZZahRMn_YNYoS4qVS2ChbrpjRw8QNEZeZ_UngAKpIzGpePPewNFpFKyePsmOQEHexEwmpVgedh2HDv2IOLvvVk428JW19W0-IMDHHgXXRPd_nRqrRe7AFsyTBJiQOo9hxLUXmhmw3O9c07yfEjhe7IwYlmO7UB8KiN2SHWVpdQZPDwq7xwl-FrAZnlE7SswP7h8tndaaqdI_oq5Y8V9IFXf6XsjDG2_aJ74Ik_IrsuTBCh5eQy7KaV5V9hVZZw39k1pw-hBU4nSVdsI0G5gI0iSPr35F034a0UsbZgJXg8kngWaKkmFtFMtBznpEP_m2W1rAgzMP-iqSrjb7D9ghUgkkAMwE1CekU1cIykKrnyf46mRJRaVNM6VWBDiXC-j9WwhMgSx7MpaLqpwlilNdng7I5A6VKCXpZM1td7V9su7SKBcTf72IfmrDznw4kg5OyuQa9o5v8wQ81m_c9RtFLbXG2Kt88D7sR3p0U3AyLJqb3hRFcWjKrANmarsp2zWG09d4yhBypnJ6JuqD06MZnIs-cX3vAUJ0ewOsUPRB75rxRsvBj24WPeMlL_NYs1QNsKN_oHqo2vEAKb4mEiPddf3yK4OLe0E87JLjEBBEADOD0A8tIVJZUSpoPGhLXR2fynFldgHgEzXk2MWL8eDZjvuWRasJfL02L7GFOIpLE2hY72PfCQevXfF8pT6ak9LXBEY2saltcAC0jfT2OIiasmw4pXrgN4sMUD2ta4PPVmnMeTyTEV3pDk-WerXRbFUx0f-c_1jfsKxR9sBDoTOHenM3WmGtuBB_sEhlgtnuPj92FDnCpKlBtkJftUcAuBRk6LHl9RL5VCTnwhhTEq072n1wWyUCNp8eXdHV6En7BSnvfnnah1ACH3A5HH0eAIzqvRAyunaedqQ9NM0f_1FAqek2k4xX_IBIVut0tF6Mv9zt1rVXk5btvrtTzVFxFwcYV8jgjdBr6L5Wi6GRifp5GaveDJ3R_90FzijaoKSECmfOCG3xPMhL5EmzIEtGdzBsj8cC6N8h0xsqpYpFHQios0q_9dZ-LUw0YQRW_xeCsva5eXaYHBnM78eAIVEffXj5cJSkW2jBX70wHQkMvXkpLx_UzA8zYvxS0NJ0MJTV1FTD4gJyCB5J1-aFmOA0XMurCzIb18_-iWPTa-BpMuLWHgzbLw5-tcu4CV2oImmT4DcvRxBNJzfNHsYlxbg-W9qG-1CcuoL99I=`:
      relative URL without a base
```

---

_Comment by @zanieb on 2025-04-22 03:38_

Awesome thank you! That confirms my suspicion we aren't handling relative URLs. A fix is in progress at #13040 — but I'll probably revert the regression (https://github.com/astral-sh/uv/pull/13041) and roll that the fix once we know it's correct.

---

_Assigned to @zanieb by @zanieb on 2025-04-22 03:57_

---

_Assigned to @jtfmumm by @zanieb on 2025-04-22 03:57_

---

_Comment by @zanieb on 2025-04-22 04:20_

0.6.16 is out now, we'll use this issue to track the fix for the bug regardless.

---

_Referenced in [astral-sh/uv#13050](../../astral-sh/uv/pulls/13050.md) on 2025-04-22 13:45_

---

_Closed by @jtfmumm on 2025-04-28 07:07_

---

_Closed by @jtfmumm on 2025-04-28 07:07_

---

_Referenced in [astral-sh/uv#13255](../../astral-sh/uv/issues/13255.md) on 2025-05-01 18:26_

---

_Comment by @jtfmumm on 2025-06-16 12:18_

We have a new fix for this problem on #13754. It would be very helpful if anyone would be willing to test that PR on their own private index. You could build that PR or you could also use:

```
uvx -v --from "uv @ git+https://github.com/astral-sh/uv@jtfm/update-redirect-handling" uv add <pkg-from-private-index> --default-index https://<username>:<password>@<private-index-url>
```

filling in `<pkg-from-private-index>`, `<username>`, `<password>`, and `<private-index-url>` with your own values.

---
