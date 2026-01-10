```yaml
number: 4696
title: Drop wheels with incompatible tag incompatible to requires-python from lockfile
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - preview
assignees: []
created_at: 2024-07-01T13:50:39Z
updated_at: 2024-07-09T10:02:29Z
url: https://github.com/astral-sh/uv/issues/4696
synced_at: 2026-01-10T05:31:37Z
```

# Drop wheels with incompatible tag incompatible to requires-python from lockfile

---

_Issue opened by @konstin on 2024-07-01 13:50_

We currently store all wheels (and the source distribution) in `uv.lock`. We should remove those that can never be installed. One clear target is removing those with an incompatible python version, such as a `cp311` tag for `requires-python = ">=3.12"`. Example:

```toml
[project]
name = "a"
version = "1.0.0"
requires-python = "==3.12.*"
dependencies = ["charset-normalizer==3.3.1"]
```

Current lockfile:

```toml
version = 1
requires-python = ">=3.12.dev0, <3.13.dev0"

[[distribution]]
name = "a"
version = "1.0.0"
source = { editable = "." }
dependencies = [
    { name = "charset-normalizer" },
]

[[distribution]]
name = "charset-normalizer"
version = "3.3.1"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/6d/b3/aa417b4e3ace24067f243e45cceaffc12dba6b8bd50c229b43b3b163768b/charset-normalizer-3.3.1.tar.gz", hash = "sha256:d9137a876020661972ca6eec0766d81aef8a5627df628b664b234b73396e727e", size = 104095 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/ac/ea/52cd06d11dbff220a464f11490d2d7e992691f1e7c3b9af614a341e58c3d/charset_normalizer-3.3.1-cp310-cp310-macosx_10_9_universal2.whl", hash = "sha256:8aee051c89e13565c6bd366813c386939f8e928af93c29fda4af86d25b73d8f8", size = 190006 },
    { url = "https://files.pythonhosted.org/packages/96/30/89222634b7887570d5d4daed6771f9b74975f1865cc68184c88b39455689/charset_normalizer-3.3.1-cp310-cp310-macosx_10_9_x86_64.whl", hash = "sha256:352a88c3df0d1fa886562384b86f9a9e27563d4704ee0e9d56ec6fcd270ea690", size = 120162 },
    { url = "https://files.pythonhosted.org/packages/24/d0/6727c243149e1e02132bab404ac9aa9e230faf5de238bfcd144f9df09f59/charset_normalizer-3.3.1-cp310-cp310-macosx_11_0_arm64.whl", hash = "sha256:223b4d54561c01048f657fa6ce41461d5ad8ff128b9678cfe8b2ecd951e3f8a2", size = 118171 },
    { url = "https://files.pythonhosted.org/packages/7e/cf/b8ff10991e913723c9e7fc8b0d9559fc78750bbb9bfe66c75886b827eec4/charset_normalizer-3.3.1-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:4f861d94c2a450b974b86093c6c027888627b8082f1299dfd5a4bae8e2292821", size = 135681 },
    { url = "https://files.pythonhosted.org/packages/ab/4c/834632ee9abcbaa90c741f92fe8bd1eabb749a72f5180f60cfd7a8480edd/charset_normalizer-3.3.1-cp310-cp310-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl", hash = "sha256:1171ef1fc5ab4693c5d151ae0fdad7f7349920eabbaca6271f95969fa0756c2d", size = 145107 },
    { url = "https://files.pythonhosted.org/packages/b9/2e/738cb7eff4f0c0f6297bac84cebea7495742cd18c3eecfe2be3aa33a8ece/charset_normalizer-3.3.1-cp310-cp310-manylinux_2_17_s390x.manylinux2014_s390x.whl", hash = "sha256:28f512b9a33235545fbbdac6a330a510b63be278a50071a336afc1b78781b147", size = 137856 },
    { url = "https://files.pythonhosted.org/packages/87/80/f0974891fdd2e756f3f4941cfca870826ba0260752ee3dc28dee4af7e401/charset_normalizer-3.3.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:c0e842112fe3f1a4ffcf64b06dc4c61a88441c2f02f373367f7b4c1aa9be2ad5", size = 139172 },
    { url = "https://files.pythonhosted.org/packages/d2/d0/7db503c1b052cdf9484eb0196470720a2f468466b9c29ccd0eb2580cee77/charset_normalizer-3.3.1-cp310-cp310-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:3f9bc2ce123637a60ebe819f9fccc614da1bcc05798bbbaf2dd4ec91f3e08846", size = 141687 },
    { url = "https://files.pythonhosted.org/packages/76/2a/a614ddc52be5802b9bc99c5f0b29ab8e9699007817204c607d3d48a990da/charset_normalizer-3.3.1-cp310-cp310-musllinux_1_1_aarch64.whl", hash = "sha256:f194cce575e59ffe442c10a360182a986535fd90b57f7debfaa5c845c409ecc3", size = 136721 },
    { url = "https://files.pythonhosted.org/packages/9c/8f/6e1bb9654cc3654911218cadd0cf8f35b988f61b38df39a63bfdd733396f/charset_normalizer-3.3.1-cp310-cp310-musllinux_1_1_i686.whl", hash = "sha256:9a74041ba0bfa9bc9b9bb2cd3238a6ab3b7618e759b41bd15b5f6ad958d17605", size = 142454 },
    { url = "https://files.pythonhosted.org/packages/f3/c2/33f569d2cefdfe84a8cc5bb2a8ea6cfd246d85fc235ae1e9220b441280d3/charset_normalizer-3.3.1-cp310-cp310-musllinux_1_1_ppc64le.whl", hash = "sha256:b578cbe580e3b41ad17b1c428f382c814b32a6ce90f2d8e39e2e635d49e498d1", size = 146851 },
    { url = "https://files.pythonhosted.org/packages/6b/9c/ca7deefa550fc149192d08aea8a79de48b7b8f396bb9078f7070e70f9fe4/charset_normalizer-3.3.1-cp310-cp310-musllinux_1_1_s390x.whl", hash = "sha256:6db3cfb9b4fcecb4390db154e75b49578c87a3b9979b40cdf90d7e4b945656e1", size = 138681 },
    { url = "https://files.pythonhosted.org/packages/3a/75/db30b8e98113a60bd3c5cd551867d4155d0a4ac4e35b451a5d268a430455/charset_normalizer-3.3.1-cp310-cp310-musllinux_1_1_x86_64.whl", hash = "sha256:debb633f3f7856f95ad957d9b9c781f8e2c6303ef21724ec94bea2ce2fcbd056", size = 139728 },
    { url = "https://files.pythonhosted.org/packages/e7/89/0f50a2ac4dc37076391964bc594db7109c59c25e8575eab414d3a22216c6/charset_normalizer-3.3.1-cp310-cp310-win32.whl", hash = "sha256:87071618d3d8ec8b186d53cb6e66955ef2a0e4fa63ccd3709c0c90ac5a43520f", size = 91491 },
    { url = "https://files.pythonhosted.org/packages/e1/73/0ea41b02dab67154b0d3fcc979194b8e08be12e4f6d17d92a6d967c25378/charset_normalizer-3.3.1-cp310-cp310-win_amd64.whl", hash = "sha256:e372d7dfd154009142631de2d316adad3cc1c36c32a38b16a4751ba78da2a397", size = 98702 },
    { url = "https://files.pythonhosted.org/packages/f4/db/048bf61f44c21287509d60bbe394f35f93b7db14ade99b8f5f9035ef04fc/charset_normalizer-3.3.1-cp311-cp311-macosx_10_9_universal2.whl", hash = "sha256:ae4070f741f8d809075ef697877fd350ecf0b7c5837ed68738607ee0a2c572cf", size = 187581 },
    { url = "https://files.pythonhosted.org/packages/ec/e9/5fe55dbe2204271ea8d6e1434af7d2067770364360b1fbeaa9cd4b8b4c47/charset_normalizer-3.3.1-cp311-cp311-macosx_10_9_x86_64.whl", hash = "sha256:58e875eb7016fd014c0eea46c6fa92b87b62c0cb31b9feae25cbbe62c919f54d", size = 119072 },
    { url = "https://files.pythonhosted.org/packages/49/48/b89a9ccc78ea7a2a0b37c20a912b98c840210f277747e2380ee8d72784cc/charset_normalizer-3.3.1-cp311-cp311-macosx_11_0_arm64.whl", hash = "sha256:dbd95e300367aa0827496fe75a1766d198d34385a58f97683fe6e07f89ca3e3c", size = 116809 },
    { url = "https://files.pythonhosted.org/packages/5d/b9/1972e394c367556c6e12739ed5f98ddba6ea1b51095b593c2b3eda8ef76e/charset_normalizer-3.3.1-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:de0b4caa1c8a21394e8ce971997614a17648f94e1cd0640fbd6b4d14cab13a72", size = 134164 },
    { url = "https://files.pythonhosted.org/packages/6e/a5/87ccac8092c29f657181a92240a5113691f802fe9fda36cba34a402563e0/charset_normalizer-3.3.1-cp311-cp311-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl", hash = "sha256:985c7965f62f6f32bf432e2681173db41336a9c2611693247069288bcb0c7f8b", size = 143695 },
    { url = "https://files.pythonhosted.org/packages/5e/58/0aea72c42480fa5cd5fcf681b9e3f650456a690b3557f85e3ff8a6db4e4c/charset_normalizer-3.3.1-cp311-cp311-manylinux_2_17_s390x.manylinux2014_s390x.whl", hash = "sha256:a15c1fe6d26e83fd2e5972425a772cca158eae58b05d4a25a4e474c221053e2d", size = 136591 },
    { url = "https://files.pythonhosted.org/packages/ae/e5/8c290f1dd50aae55d1ec20420a6df3c051d6f5ad78ee5b88b1a7ef26634b/charset_normalizer-3.3.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:ae55d592b02c4349525b6ed8f74c692509e5adffa842e582c0f861751701a673", size = 137510 },
    { url = "https://files.pythonhosted.org/packages/dc/9b/b28dd88e6f3e5fb231d2fcd43660047aa055feceadbafaa8d9d10ea8c48e/charset_normalizer-3.3.1-cp311-cp311-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:be4d9c2770044a59715eb57c1144dedea7c5d5ae80c68fb9959515037cde2008", size = 139858 },
    { url = "https://files.pythonhosted.org/packages/8f/6c/e6258afa32fcfe58c24b7ac80f2499f0683999924f43b439be40f040266f/charset_normalizer-3.3.1-cp311-cp311-musllinux_1_1_aarch64.whl", hash = "sha256:851cf693fb3aaef71031237cd68699dded198657ec1e76a76eb8be58c03a5d1f", size = 135095 },
    { url = "https://files.pythonhosted.org/packages/d1/95/ddcab18a631f3705248e5027b8f6e54aba7bbdd64d19f6f7db951cda54b9/charset_normalizer-3.3.1-cp311-cp311-musllinux_1_1_i686.whl", hash = "sha256:31bbaba7218904d2eabecf4feec0d07469284e952a27400f23b6628439439fa7", size = 140760 },
    { url = "https://files.pythonhosted.org/packages/4f/72/1b5ddf63cb0dcb1748068fc6aba498b72513b17969adaf0dd978b6afe46b/charset_normalizer-3.3.1-cp311-cp311-musllinux_1_1_ppc64le.whl", hash = "sha256:871d045d6ccc181fd863a3cd66ee8e395523ebfbc57f85f91f035f50cee8e3d4", size = 145476 },
    { url = "https://files.pythonhosted.org/packages/89/28/5da57065951f04269c69b8eba0546f6f5b1fb1c0207714f3c3b30732727b/charset_normalizer-3.3.1-cp311-cp311-musllinux_1_1_s390x.whl", hash = "sha256:501adc5eb6cd5f40a6f77fbd90e5ab915c8fd6e8c614af2db5561e16c600d6f3", size = 137461 },
    { url = "https://files.pythonhosted.org/packages/ea/11/e2908ae0f5812d054350f32d32734194c3d0677b2f676d3580a81a3d73c1/charset_normalizer-3.3.1-cp311-cp311-musllinux_1_1_x86_64.whl", hash = "sha256:f5fb672c396d826ca16a022ac04c9dce74e00a1c344f6ad1a0fdc1ba1f332213", size = 138003 },
    { url = "https://files.pythonhosted.org/packages/7a/1d/2f1a3bd50ac35135b3b8bce327e21a2afcaeed93747b2f24922207b83cac/charset_normalizer-3.3.1-cp311-cp311-win32.whl", hash = "sha256:bb06098d019766ca16fc915ecaa455c1f1cd594204e7f840cd6258237b5079a8", size = 91204 },
    { url = "https://files.pythonhosted.org/packages/ef/96/1c7e85db0a1b2f182d47375987e82aacb60c987e3943b11ccce3fc6aebab/charset_normalizer-3.3.1-cp311-cp311-win_amd64.whl", hash = "sha256:8af5a8917b8af42295e86b64903156b4f110a30dca5f3b5aedea123fbd638bff", size = 98322 },
    { url = "https://files.pythonhosted.org/packages/95/7b/191f93d117d914b55baa2f06b8a08eb9e7858774c0ddf2c2a8b68330cee6/charset_normalizer-3.3.1-cp312-cp312-macosx_10_9_universal2.whl", hash = "sha256:7ae8e5142dcc7a49168f4055255dbcced01dc1714a90a21f87448dc8d90617d1", size = 188666 },
    { url = "https://files.pythonhosted.org/packages/be/d0/a14afd69344d248d7e8ab495b44be6957a6fd3718ffeb6b48674f206e8fa/charset_normalizer-3.3.1-cp312-cp312-macosx_10_9_x86_64.whl", hash = "sha256:5b70bab78accbc672f50e878a5b73ca692f45f5b5e25c8066d748c09405e6a55", size = 119936 },
    { url = "https://files.pythonhosted.org/packages/d4/85/5fdf066f9eb035cac957e89f90508a2698d1c8bd93d6a6477ab449e72cdc/charset_normalizer-3.3.1-cp312-cp312-macosx_11_0_arm64.whl", hash = "sha256:5ceca5876032362ae73b83347be8b5dbd2d1faf3358deb38c9c88776779b2e2f", size = 117166 },
    { url = "https://files.pythonhosted.org/packages/bc/91/b90b70780b2481247d53bdd3657487005affa804418cefa5fba303909985/charset_normalizer-3.3.1-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:34d95638ff3613849f473afc33f65c401a89f3b9528d0d213c7037c398a51296", size = 134804 },
    { url = "https://files.pythonhosted.org/packages/65/96/2c5d1e789967610eb31d3babd10072bc2e0e6467efa008e06d9cadebac44/charset_normalizer-3.3.1-cp312-cp312-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl", hash = "sha256:9edbe6a5bf8b56a4a84533ba2b2f489d0046e755c29616ef8830f9e7d9cf5728", size = 144436 },
    { url = "https://files.pythonhosted.org/packages/e3/43/7d932d0a52ba4a9ae6c2c5f1a17bfae553595587de184522dc154727de85/charset_normalizer-3.3.1-cp312-cp312-manylinux_2_17_s390x.manylinux2014_s390x.whl", hash = "sha256:f6a02a3c7950cafaadcd46a226ad9e12fc9744652cc69f9e5534f98b47f3bbcf", size = 137368 },
    { url = "https://files.pythonhosted.org/packages/93/6d/63027361182c26155517ed010a05d73528511f45faab2047a014e69c4651/charset_normalizer-3.3.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:10b8dd31e10f32410751b3430996f9807fc4d1587ca69772e2aa940a82ab571a", size = 139124 },
    { url = "https://files.pythonhosted.org/packages/c5/4d/3b0f81da0011755a0d796eff2ff36cda713b0fdc913d93cca121f623e703/charset_normalizer-3.3.1-cp312-cp312-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:edc0202099ea1d82844316604e17d2b175044f9bcb6b398aab781eba957224bd", size = 141305 },
    { url = "https://files.pythonhosted.org/packages/26/a4/086415b5c422527bf046eb66ee7f4605bc9218464703cf6dd4f2a3908a49/charset_normalizer-3.3.1-cp312-cp312-musllinux_1_1_aarch64.whl", hash = "sha256:b891a2f68e09c5ef989007fac11476ed33c5c9994449a4e2c3386529d703dc8b", size = 135484 },
    { url = "https://files.pythonhosted.org/packages/58/36/0fa50b4c5e0fb8633b725389835084adc12bf102de666233413012b538ce/charset_normalizer-3.3.1-cp312-cp312-musllinux_1_1_i686.whl", hash = "sha256:71ef3b9be10070360f289aea4838c784f8b851be3ba58cf796262b57775c2f14", size = 142103 },
    { url = "https://files.pythonhosted.org/packages/ab/ae/0ce19f42cc559c610e38dfa9deed259b2ced0b22b1f3e3a4fa2437e2801b/charset_normalizer-3.3.1-cp312-cp312-musllinux_1_1_ppc64le.whl", hash = "sha256:55602981b2dbf8184c098bc10287e8c245e351cd4fdcad050bd7199d5a8bf514", size = 146388 },
    { url = "https://files.pythonhosted.org/packages/2c/ed/614664c8c8cfcdbfb883886a07adfb11113d8d7318a96d3da6ecdda2acdc/charset_normalizer-3.3.1-cp312-cp312-musllinux_1_1_s390x.whl", hash = "sha256:46fb9970aa5eeca547d7aa0de5d4b124a288b42eaefac677bde805013c95725c", size = 138315 },
    { url = "https://files.pythonhosted.org/packages/30/c8/083fdaa56751ce753d16594973cbfe838b67b24b448aaac7ca3298eb7a70/charset_normalizer-3.3.1-cp312-cp312-musllinux_1_1_x86_64.whl", hash = "sha256:520b7a142d2524f999447b3a0cf95115df81c4f33003c51a6ab637cbda9d0bf4", size = 139231 },
    { url = "https://files.pythonhosted.org/packages/6d/75/405dfa90cb7afcc8b3aa35e8da1aa9a0b69743263872d762f29c6a435a68/charset_normalizer-3.3.1-cp312-cp312-win32.whl", hash = "sha256:8ec8ef42c6cd5856a7613dcd1eaf21e5573b2185263d87d27c8edcae33b62a61", size = 91786 },
    { url = "https://files.pythonhosted.org/packages/b2/a5/d52c61864fea4b02ef11da0a2e829ea7b365e09fae2a132314541ad01ae4/charset_normalizer-3.3.1-cp312-cp312-win_amd64.whl", hash = "sha256:baec8148d6b8bd5cee1ae138ba658c71f5b03e0d69d5907703e3e1df96db5e41", size = 98756 },
    { url = "https://files.pythonhosted.org/packages/65/24/d243ee1264a55212b19031c9c1361d6a2eec42ef2795cfa24f43eb5b0ef2/charset_normalizer-3.3.1-cp37-cp37m-macosx_10_9_x86_64.whl", hash = "sha256:63a6f59e2d01310f754c270e4a257426fe5a591dc487f1983b3bbe793cf6bac6", size = 116223 },
    { url = "https://files.pythonhosted.org/packages/28/da/b682d1079ef8f040b117145317ba4a5a9f116658873abbbcc64afcf9d16b/charset_normalizer-3.3.1-cp37-cp37m-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:1d6bfc32a68bc0933819cfdfe45f9abc3cae3877e1d90aac7259d57e6e0f85b1", size = 131113 },
    { url = "https://files.pythonhosted.org/packages/bd/a1/fa4e3f11373850a6c5835deb0b9929af65661534313c012c3bdb0e56a67c/charset_normalizer-3.3.1-cp37-cp37m-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl", hash = "sha256:4f3100d86dcd03c03f7e9c3fdb23d92e32abbca07e7c13ebd7ddfbcb06f5991f", size = 140066 },
    { url = "https://files.pythonhosted.org/packages/a7/e2/974c0fd1dc9e6a6d925676e09e706f3fddcf9e557d36759e3e4fce2378a5/charset_normalizer-3.3.1-cp37-cp37m-manylinux_2_17_s390x.manylinux2014_s390x.whl", hash = "sha256:39b70a6f88eebe239fa775190796d55a33cfb6d36b9ffdd37843f7c4c1b5dc67", size = 133300 },
    { url = "https://files.pythonhosted.org/packages/08/de/d100c66a901bd668ae1576f8b59c6074f27aa20a356dfd2ba2f2d241089b/charset_normalizer-3.3.1-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:4e12f8ee80aa35e746230a2af83e81bd6b52daa92a8afaef4fea4a2ce9b9f4fa", size = 134048 },
    { url = "https://files.pythonhosted.org/packages/d0/02/440d301a99a9703b33d7e56d0b804b89d0ece08633f1de13314982b008b5/charset_normalizer-3.3.1-cp37-cp37m-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:7b6cefa579e1237ce198619b76eaa148b71894fb0d6bcf9024460f9bf30fd228", size = 136915 },
    { url = "https://files.pythonhosted.org/packages/81/5c/87bbec66762dcf3fb823ae20821fb6d41eb0de17d3cc5996fe735265a46b/charset_normalizer-3.3.1-cp37-cp37m-musllinux_1_1_aarch64.whl", hash = "sha256:61f1e3fb621f5420523abb71f5771a204b33c21d31e7d9d86881b2cffe92c47c", size = 131526 },
    { url = "https://files.pythonhosted.org/packages/57/e7/1752a432d6191b4900094291ec937e16f1d73cda09093dfc06843aa7ebe5/charset_normalizer-3.3.1-cp37-cp37m-musllinux_1_1_i686.whl", hash = "sha256:4f6e2a839f83a6a76854d12dbebde50e4b1afa63e27761549d006fa53e9aa80e", size = 137555 },
    { url = "https://files.pythonhosted.org/packages/f6/37/db71710eb38793b4471008bbe6f455dc3b652e5b39aa88c9ad4e97fd7c3b/charset_normalizer-3.3.1-cp37-cp37m-musllinux_1_1_ppc64le.whl", hash = "sha256:1ec937546cad86d0dce5396748bf392bb7b62a9eeb8c66efac60e947697f0e58", size = 141430 },
    { url = "https://files.pythonhosted.org/packages/cc/e5/1f6fdafed43df36a0e178e66b2df4ac5fbb1ee80a03edc7212cf77dad678/charset_normalizer-3.3.1-cp37-cp37m-musllinux_1_1_s390x.whl", hash = "sha256:82ca51ff0fc5b641a2d4e1cc8c5ff108699b7a56d7f3ad6f6da9dbb6f0145b48", size = 134008 },
    { url = "https://files.pythonhosted.org/packages/f4/3d/cc4f99f3a592e89641f04c05c0bd7c9179a175d422b20be9d3f764c179e3/charset_normalizer-3.3.1-cp37-cp37m-musllinux_1_1_x86_64.whl", hash = "sha256:633968254f8d421e70f91c6ebe71ed0ab140220469cf87a9857e21c16687c034", size = 134193 },
    { url = "https://files.pythonhosted.org/packages/78/50/d06341b5b982e67ce64f8da8e63841f985b73d32eb2b0f3bc31b7614a64e/charset_normalizer-3.3.1-cp37-cp37m-win32.whl", hash = "sha256:c0c72d34e7de5604df0fde3644cc079feee5e55464967d10b24b1de268deceb9", size = 90068 },
    { url = "https://files.pythonhosted.org/packages/a0/e4/0e1e212e3ec93e1aa86a02e9577d96c10918caee3fd4693a0b56dc99ac99/charset_normalizer-3.3.1-cp37-cp37m-win_amd64.whl", hash = "sha256:63accd11149c0f9a99e3bc095bbdb5a464862d77a7e309ad5938fbc8721235ae", size = 96573 },
    { url = "https://files.pythonhosted.org/packages/e1/d0/c0a6cbc1ac333b28ecdd9385c10b418d159000de25efc2983abd4903600f/charset_normalizer-3.3.1-cp38-cp38-macosx_10_9_universal2.whl", hash = "sha256:5a3580a4fdc4ac05f9e53c57f965e3594b2f99796231380adb2baaab96e22761", size = 187278 },
    { url = "https://files.pythonhosted.org/packages/76/05/cba1845a556baa96889d48ec9fe16579560df53eecd3624d0ba563a26186/charset_normalizer-3.3.1-cp38-cp38-macosx_10_9_x86_64.whl", hash = "sha256:2465aa50c9299d615d757c1c888bc6fef384b7c4aec81c05a0172b4400f98557", size = 118661 },
    { url = "https://files.pythonhosted.org/packages/41/1f/eb105c3ac04a4582edd45311af67c7afe556a5c2a7538215586d82bfb176/charset_normalizer-3.3.1-cp38-cp38-macosx_11_0_arm64.whl", hash = "sha256:cb7cd68814308aade9d0c93c5bd2ade9f9441666f8ba5aa9c2d4b389cb5e2a45", size = 116888 },
    { url = "https://files.pythonhosted.org/packages/b4/c1/675fb61cffeb586afd51c04d9c20ceb8a0af55c09b0d42a07247147f1313/charset_normalizer-3.3.1-cp38-cp38-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:91e43805ccafa0a91831f9cd5443aa34528c0c3f2cc48c4cb3d9a7721053874b", size = 134974 },
    { url = "https://files.pythonhosted.org/packages/a8/12/baf65de35ecb877070dc7147469f761a12ff28eef667e7b4f9c7007fc4c4/charset_normalizer-3.3.1-cp38-cp38-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl", hash = "sha256:854cc74367180beb327ab9d00f964f6d91da06450b0855cbbb09187bcdb02de5", size = 144173 },
    { url = "https://files.pythonhosted.org/packages/f4/b5/1efac062db7abeceb75edee97293bb0b0e4b46b9f58c78df874aee8db22c/charset_normalizer-3.3.1-cp38-cp38-manylinux_2_17_s390x.manylinux2014_s390x.whl", hash = "sha256:c15070ebf11b8b7fd1bfff7217e9324963c82dbdf6182ff7050519e350e7ad9f", size = 136967 },
    { url = "https://files.pythonhosted.org/packages/9f/07/ffb69702716514cca44d58c7cd4f10fcc81e8a44a0e95bd8fd188a709a80/charset_normalizer-3.3.1-cp38-cp38-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:2c4c99f98fc3a1835af8179dcc9013f93594d0670e2fa80c83aa36346ee763d2", size = 138285 },
    { url = "https://files.pythonhosted.org/packages/45/cd/ce60ae86f081d304f5539f03b2738368bde8a7266241450f79507a61b59c/charset_normalizer-3.3.1-cp38-cp38-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:3fb765362688821404ad6cf86772fc54993ec11577cd5a92ac44b4c2ba52155b", size = 141276 },
    { url = "https://files.pythonhosted.org/packages/df/5e/1dec01614ff5517dcbcffad14c141aafcb80b0b3eb8bb49d650acbd43f0b/charset_normalizer-3.3.1-cp38-cp38-musllinux_1_1_aarch64.whl", hash = "sha256:dced27917823df984fe0c80a5c4ad75cf58df0fbfae890bc08004cd3888922a2", size = 135745 },
    { url = "https://files.pythonhosted.org/packages/b9/de/5658ca5042639ce4936124ba168ca6fb2a945c3e59b2083b3ea3881cd2ad/charset_normalizer-3.3.1-cp38-cp38-musllinux_1_1_i686.whl", hash = "sha256:a66bcdf19c1a523e41b8e9d53d0cedbfbac2e93c649a2e9502cb26c014d0980c", size = 142179 },
    { url = "https://files.pythonhosted.org/packages/0f/83/0deeb4c098774f5c9fad070125e630dfe4f2a0aa4f895448d959535a0340/charset_normalizer-3.3.1-cp38-cp38-musllinux_1_1_ppc64le.whl", hash = "sha256:ecd26be9f112c4f96718290c10f4caea6cc798459a3a76636b817a0ed7874e42", size = 145865 },
    { url = "https://files.pythonhosted.org/packages/f4/54/ff0d0c12fc369f2da4c006ba4a82ab6ef7fb96693d5574523cb52705dbbb/charset_normalizer-3.3.1-cp38-cp38-musllinux_1_1_s390x.whl", hash = "sha256:3f70fd716855cd3b855316b226a1ac8bdb3caf4f7ea96edcccc6f484217c9597", size = 137729 },
    { url = "https://files.pythonhosted.org/packages/b5/23/63008aa7ab7537ca4c6873d929716698f61aeb7f87a7a567f1290592ed2c/charset_normalizer-3.3.1-cp38-cp38-musllinux_1_1_x86_64.whl", hash = "sha256:17a866d61259c7de1bdadef418a37755050ddb4b922df8b356503234fff7932c", size = 138412 },
    { url = "https://files.pythonhosted.org/packages/3a/88/1205ce879242ed7776c91ce99d8e71b5c560fd7f88c84cf62d746c73b5c9/charset_normalizer-3.3.1-cp38-cp38-win32.whl", hash = "sha256:548eefad783ed787b38cb6f9a574bd8664468cc76d1538215d510a3cd41406cb", size = 91037 },
    { url = "https://files.pythonhosted.org/packages/43/f4/47d702f31198ec1e29333a3a4aa032eb6b486274083e43edc87d992a76be/charset_normalizer-3.3.1-cp38-cp38-win_amd64.whl", hash = "sha256:45f053a0ece92c734d874861ffe6e3cc92150e32136dd59ab1fb070575189c97", size = 97990 },
    { url = "https://files.pythonhosted.org/packages/e4/30/d317a00b759e3f8468f6d4659729113094a9a062b81e0e10dfeb9440a3b7/charset_normalizer-3.3.1-cp39-cp39-macosx_10_9_universal2.whl", hash = "sha256:bc791ec3fd0c4309a753f95bb6c749ef0d8ea3aea91f07ee1cf06b7b02118f2f", size = 189988 },
    { url = "https://files.pythonhosted.org/packages/fb/ab/564369d80e72be59fd7cc5392a45dbbbbad0495fbb131d6144835c9b2066/charset_normalizer-3.3.1-cp39-cp39-macosx_10_9_x86_64.whl", hash = "sha256:0c8c61fb505c7dad1d251c284e712d4e0372cef3b067f7ddf82a7fa82e1e9a93", size = 120149 },
    { url = "https://files.pythonhosted.org/packages/18/2d/ac1866113e1ca6917be0420000d4b1955a79665f2959803eb7a785a18d44/charset_normalizer-3.3.1-cp39-cp39-macosx_11_0_arm64.whl", hash = "sha256:2c092be3885a1b7899cd85ce24acedc1034199d6fca1483fa2c3a35c86e43041", size = 118150 },
    { url = "https://files.pythonhosted.org/packages/97/f5/43fdadb5ce51f5fb6b46b829100c6a229411ff2fc8a46c80b7e423353a35/charset_normalizer-3.3.1-cp39-cp39-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:c2000c54c395d9e5e44c99dc7c20a64dc371f777faf8bae4919ad3e99ce5253e", size = 135808 },
    { url = "https://files.pythonhosted.org/packages/79/30/6c234d5bec08768405508ff759fefdd6fdd943d2ed5859c928d296688de2/charset_normalizer-3.3.1-cp39-cp39-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl", hash = "sha256:4cb50a0335382aac15c31b61d8531bc9bb657cfd848b1d7158009472189f3d62", size = 145059 },
    { url = "https://files.pythonhosted.org/packages/8f/b6/7d9412ace6d9299f8affc75a2e92d6fed656ad63fffec600f9d6fa4a72c8/charset_normalizer-3.3.1-cp39-cp39-manylinux_2_17_s390x.manylinux2014_s390x.whl", hash = "sha256:c30187840d36d0ba2893bc3271a36a517a717f9fd383a98e2697ee890a37c273", size = 137980 },
    { url = "https://files.pythonhosted.org/packages/25/ba/fb6d43cbc05269b3e3f6c811b33307e2a31bb893287bda9407996e4fe969/charset_normalizer-3.3.1-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:fe81b35c33772e56f4b6cf62cf4aedc1762ef7162a31e6ac7fe5e40d0149eb67", size = 139453 },
    { url = "https://files.pythonhosted.org/packages/22/ef/0e6fad1ea6ef590e0524436f2c843bf64d19db7601a6699289a7b5e1b52d/charset_normalizer-3.3.1-cp39-cp39-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:d0bf89afcbcf4d1bb2652f6580e5e55a840fdf87384f6063c4a4f0c95e378656", size = 141781 },
    { url = "https://files.pythonhosted.org/packages/41/d5/b94bd20c3695dfe84d5930b04331a2325a827927077308c290586b1a832b/charset_normalizer-3.3.1-cp39-cp39-musllinux_1_1_aarch64.whl", hash = "sha256:06cf46bdff72f58645434d467bf5228080801298fbba19fe268a01b4534467f5", size = 136786 },
    { url = "https://files.pythonhosted.org/packages/df/05/81c41da39121d82f4bf3cbfe57c3b2553f30d41272cca30fc90ebf3ace54/charset_normalizer-3.3.1-cp39-cp39-musllinux_1_1_i686.whl", hash = "sha256:3c66df3f41abee950d6638adc7eac4730a306b022570f71dd0bd6ba53503ab57", size = 142730 },
    { url = "https://files.pythonhosted.org/packages/84/9e/0765ddd7b3ac7f8ea78feedf28aac271134bebcb5b83c62bd66ef2f018dd/charset_normalizer-3.3.1-cp39-cp39-musllinux_1_1_ppc64le.whl", hash = "sha256:cd805513198304026bd379d1d516afbf6c3c13f4382134a2c526b8b854da1c2e", size = 146780 },
    { url = "https://files.pythonhosted.org/packages/63/28/c548c2a103a0b8880a4a1f2664491e2719e3407abd55a2b4c8f067fb885b/charset_normalizer-3.3.1-cp39-cp39-musllinux_1_1_s390x.whl", hash = "sha256:9505dc359edb6a330efcd2be825fdb73ee3e628d9010597aa1aee5aa63442e97", size = 138838 },
    { url = "https://files.pythonhosted.org/packages/e1/b1/dfe30188e2ecf8cf6f3e292798378ab73555891405a83fa2d2dbe98f97ad/charset_normalizer-3.3.1-cp39-cp39-musllinux_1_1_x86_64.whl", hash = "sha256:31445f38053476a0c4e6d12b047b08ced81e2c7c712e5a1ad97bc913256f91b2", size = 139769 },
    { url = "https://files.pythonhosted.org/packages/d5/bc/6f925c1be2d4fe790d1b8269f275c2180daea12540006824d01d24abbeeb/charset_normalizer-3.3.1-cp39-cp39-win32.whl", hash = "sha256:bd28b31730f0e982ace8663d108e01199098432a30a4c410d06fe08fdb9e93f4", size = 91417 },
    { url = "https://files.pythonhosted.org/packages/40/47/94dee389a507ca8526bbae312780d32660b05da9429b3483c932f1031614/charset_normalizer-3.3.1-cp39-cp39-win_amd64.whl", hash = "sha256:555fe186da0068d3354cdf4bbcbc609b0ecae4d04c921cc13e209eece7720727", size = 98749 },
    { url = "https://files.pythonhosted.org/packages/22/ac/70f41edd03346a23df001e67daffebbf74cb0ab2d2347725d633efa6d379/charset_normalizer-3.3.1-py3-none-any.whl", hash = "sha256:800561453acdecedaac137bf09cd719c7a440b6800ec182f077bb8e7025fb708", size = 48246 },
]
```

We should trim this down to the set of installable wheels  for python 3.12 to get the desired lockfile:

```toml
version = 1
requires-python = ">=3.12.dev0, <3.13.dev0"

[[distribution]]
name = "a"
version = "1.0.0"
source = { editable = "." }
dependencies = [
    { name = "charset-normalizer" },
]

[[distribution]]
name = "charset-normalizer"
version = "3.3.1"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/6d/b3/aa417b4e3ace24067f243e45cceaffc12dba6b8bd50c229b43b3b163768b/charset-normalizer-3.3.1.tar.gz", hash = "sha256:d9137a876020661972ca6eec0766d81aef8a5627df628b664b234b73396e727e", size = 104095 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/95/7b/191f93d117d914b55baa2f06b8a08eb9e7858774c0ddf2c2a8b68330cee6/charset_normalizer-3.3.1-cp312-cp312-macosx_10_9_universal2.whl", hash = "sha256:7ae8e5142dcc7a49168f4055255dbcced01dc1714a90a21f87448dc8d90617d1", size = 188666 },
    { url = "https://files.pythonhosted.org/packages/be/d0/a14afd69344d248d7e8ab495b44be6957a6fd3718ffeb6b48674f206e8fa/charset_normalizer-3.3.1-cp312-cp312-macosx_10_9_x86_64.whl", hash = "sha256:5b70bab78accbc672f50e878a5b73ca692f45f5b5e25c8066d748c09405e6a55", size = 119936 },
    { url = "https://files.pythonhosted.org/packages/d4/85/5fdf066f9eb035cac957e89f90508a2698d1c8bd93d6a6477ab449e72cdc/charset_normalizer-3.3.1-cp312-cp312-macosx_11_0_arm64.whl", hash = "sha256:5ceca5876032362ae73b83347be8b5dbd2d1faf3358deb38c9c88776779b2e2f", size = 117166 },
    { url = "https://files.pythonhosted.org/packages/bc/91/b90b70780b2481247d53bdd3657487005affa804418cefa5fba303909985/charset_normalizer-3.3.1-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:34d95638ff3613849f473afc33f65c401a89f3b9528d0d213c7037c398a51296", size = 134804 },
    { url = "https://files.pythonhosted.org/packages/65/96/2c5d1e789967610eb31d3babd10072bc2e0e6467efa008e06d9cadebac44/charset_normalizer-3.3.1-cp312-cp312-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl", hash = "sha256:9edbe6a5bf8b56a4a84533ba2b2f489d0046e755c29616ef8830f9e7d9cf5728", size = 144436 },
    { url = "https://files.pythonhosted.org/packages/e3/43/7d932d0a52ba4a9ae6c2c5f1a17bfae553595587de184522dc154727de85/charset_normalizer-3.3.1-cp312-cp312-manylinux_2_17_s390x.manylinux2014_s390x.whl", hash = "sha256:f6a02a3c7950cafaadcd46a226ad9e12fc9744652cc69f9e5534f98b47f3bbcf", size = 137368 },
    { url = "https://files.pythonhosted.org/packages/93/6d/63027361182c26155517ed010a05d73528511f45faab2047a014e69c4651/charset_normalizer-3.3.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:10b8dd31e10f32410751b3430996f9807fc4d1587ca69772e2aa940a82ab571a", size = 139124 },
    { url = "https://files.pythonhosted.org/packages/c5/4d/3b0f81da0011755a0d796eff2ff36cda713b0fdc913d93cca121f623e703/charset_normalizer-3.3.1-cp312-cp312-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:edc0202099ea1d82844316604e17d2b175044f9bcb6b398aab781eba957224bd", size = 141305 },
    { url = "https://files.pythonhosted.org/packages/26/a4/086415b5c422527bf046eb66ee7f4605bc9218464703cf6dd4f2a3908a49/charset_normalizer-3.3.1-cp312-cp312-musllinux_1_1_aarch64.whl", hash = "sha256:b891a2f68e09c5ef989007fac11476ed33c5c9994449a4e2c3386529d703dc8b", size = 135484 },
    { url = "https://files.pythonhosted.org/packages/58/36/0fa50b4c5e0fb8633b725389835084adc12bf102de666233413012b538ce/charset_normalizer-3.3.1-cp312-cp312-musllinux_1_1_i686.whl", hash = "sha256:71ef3b9be10070360f289aea4838c784f8b851be3ba58cf796262b57775c2f14", size = 142103 },
    { url = "https://files.pythonhosted.org/packages/ab/ae/0ce19f42cc559c610e38dfa9deed259b2ced0b22b1f3e3a4fa2437e2801b/charset_normalizer-3.3.1-cp312-cp312-musllinux_1_1_ppc64le.whl", hash = "sha256:55602981b2dbf8184c098bc10287e8c245e351cd4fdcad050bd7199d5a8bf514", size = 146388 },
    { url = "https://files.pythonhosted.org/packages/2c/ed/614664c8c8cfcdbfb883886a07adfb11113d8d7318a96d3da6ecdda2acdc/charset_normalizer-3.3.1-cp312-cp312-musllinux_1_1_s390x.whl", hash = "sha256:46fb9970aa5eeca547d7aa0de5d4b124a288b42eaefac677bde805013c95725c", size = 138315 },
    { url = "https://files.pythonhosted.org/packages/30/c8/083fdaa56751ce753d16594973cbfe838b67b24b448aaac7ca3298eb7a70/charset_normalizer-3.3.1-cp312-cp312-musllinux_1_1_x86_64.whl", hash = "sha256:520b7a142d2524f999447b3a0cf95115df81c4f33003c51a6ab637cbda9d0bf4", size = 139231 },
    { url = "https://files.pythonhosted.org/packages/6d/75/405dfa90cb7afcc8b3aa35e8da1aa9a0b69743263872d762f29c6a435a68/charset_normalizer-3.3.1-cp312-cp312-win32.whl", hash = "sha256:8ec8ef42c6cd5856a7613dcd1eaf21e5573b2185263d87d27c8edcae33b62a61", size = 91786 },
    { url = "https://files.pythonhosted.org/packages/b2/a5/d52c61864fea4b02ef11da0a2e829ea7b365e09fae2a132314541ad01ae4/charset_normalizer-3.3.1-cp312-cp312-win_amd64.whl", hash = "sha256:baec8148d6b8bd5cee1ae138ba658c71f5b03e0d69d5907703e3e1df96db5e41", size = 98756 },
    { url = "https://files.pythonhosted.org/packages/22/ac/70f41edd03346a23df001e67daffebbf74cb0ab2d2347725d633efa6d379/charset_normalizer-3.3.1-py3-none-any.whl", hash = "sha256:800561453acdecedaac137bf09cd719c7a440b6800ec182f077bb8e7025fb708", size = 48246 },
]
```

---

_Label `enhancement` added by @konstin on 2024-07-01 13:50_

---

_Label `preview` added by @konstin on 2024-07-01 13:50_

---

_Comment by @konstin on 2024-07-09 10:02_

Fixed by https://github.com/astral-sh/uv/pull/4799

---

_Closed by @konstin on 2024-07-09 10:02_

---
