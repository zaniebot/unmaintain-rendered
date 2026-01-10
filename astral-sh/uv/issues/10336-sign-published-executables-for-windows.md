```yaml
number: 10336
title: Sign published executables for Windows
type: issue
state: open
author: zanieb
labels:
  - windows
  - releases
assignees: []
created_at: 2025-01-06T23:03:52Z
updated_at: 2026-01-08T21:54:18Z
url: https://github.com/astral-sh/uv/issues/10336
synced_at: 2026-01-10T03:11:33Z
```

# Sign published executables for Windows

---

_Issue opened by @zanieb on 2025-01-06 23:03_

It’d be great to sign our Windows releases so we stop getting reported by antivirus software. We haven’t done much investigation yet, but requiring a physical key on release would be disappointing — I think there are alternative workflows.

---

_Label `releases` added by @zanieb on 2025-01-06 23:03_

---

_Label `windows` added by @zanieb on 2025-01-06 23:03_

---

_Comment by @shauneccles on 2025-01-07 06:16_

You can use Azure Key Vault to store keys to sign - here's a [globalsign guide on setting up AKV](https://support.globalsign.com/code-signing/Code-Signing-certificate-setup-in-Azure-Key-vault), and a similar guide for [using the key](https://support.globalsign.com/code-signing/code-signing-using-azure-key-vault)

There's an azure action for signing - https://github.com/Azure/trusted-signing-action

(not affiliated with anyone, just using resources I know about)

---

_Comment by @zanieb on 2025-01-07 17:27_

Thank you! That is the alternative I was referring to, just didn't have it off the top of my head :)

---

_Comment by @zanieb on 2025-01-07 17:28_

Related https://github.com/astral-sh/python-build-standalone/issues/89

---

_Comment by @Gankra on 2025-01-13 15:06_

I researched this a bunch a couple years ago, which is an important period because in June 2023 Windows codesigning became significantly more challenging: an agreement between all code signing cert providers went into affect and the minimum security level of (useful for Windows) code signing certs went up.

The result is that code signing certs cost hundreds of dollars a year and can *only* be issued via HSMs (yubikeys). This necessitates paying a certificate authority for the cert, and then paying a cloud services provider that has a peering agreement with them to host an HSM for you (and as of a year ago this was a tangle of bad interop stories). Alternatively you use your cert provider's cloud service where you upload files to be signed and they send them back to you (so they're hosting their own HSMs).

However last year Microsoft announced they were throwing a first-party hat into the process, which seemed cheaper and easier to use. However at the time they were quoting things like "you need 2 (3?) years of tax filings to apply for one of our certs" which bounced a lot of the folks we were working with. They've since left beta and redone all their copy so I am having trouble finding that requirement anymore, hopefully it's gone.

Useful links:

* [Incredible summary of Windows code signing infra/practicalities](https://about.signpath.io/code-signing/windows-platform)

* [Microsoft's first-party solution: Trusted Signing](https://azure.microsoft.com/en-us/products/trusted-signing)

---

_Comment by @kariharju on 2025-10-01 15:05_

Pinged in the mac signing topic as well.

We've been doing signing for our tool executables and installers for years now.
Windows EV does not cost that much and can simply be stored in Azure Vault. At the moment mac signing costs more.

Getting the EV cert. takes a while and is a process to get, but manageable.

We would be open to contribute GHA pipeline examples if needed.
This is not a thing to be afraid about, but having not-signed executables is becoming a non-starter for any kind of wide-scale enterprise us. Most antivirus tools are ML based and a un-signed executable download scripts and other executables is understandably not a good look.


---

_Comment by @zanieb on 2025-10-01 15:13_

Do you have suggestions / preferences between the vendors at https://learn.microsoft.com/en-us/windows-hardware/drivers/dashboard/code-signing-reqs#where-to-get-ev-code-signing-certificates ?

---

_Comment by @kariharju on 2025-10-01 15:21_

We went with GlobalSign, mostly because we had used that in our previous jobs.
No real preference, the prices seem to differ a lot these days (for some weird reasons)
And I guess the identity vetting process times can vary. We knew from get go that we needed to sign every single executable. Back 6 years ago this required the usb dongle and a dedicated machine.. which was .. fun 😅

Now in GHA it is just:
```
- name: Build
  run: |
    dotnet tool install --global AzureSignTool --version 3.0.0
    azuresigntool sign `
      --description-url "https://astral.sh" `
      --file-digest sha256 `
      --azure-key-vault-url $Env:VAULT_URL `
      --azure-key-vault-client-id $Env:CLIENT_ID `
      --azure-key-vault-tenant-id $Env:TENANT_ID `
      --azure-key-vault-client-secret $Env:CLIENT_SECRET `
      --azure-key-vault-certificate $Env:CERTIFICATE_NAME `
      --timestamp-rfc3161 http://timestamp.digicert.com `
      --timestamp-digest sha256 `
      <your executable>
```
..and setup the secrets.

---

_Comment by @woodruffw on 2025-10-01 18:58_

@kariharju Could you explain why an EV cert is needed here? My understanding is that Windows code signing (= Authenticode) doesn't _require_ EV or OV, i.e. we _could_ use just a basic code signing certificate here as long as it chains to one of MSFT's trusted roots. But I could be wrong about that 🙂 

I'm basing that in part on Azure's "Trusted Signing" service, which appears to offer short-lived signing certs without the EV/OV validation bits.

---

_Comment by @woodruffw on 2025-10-01 19:02_

(@Gankra just informed me that EV is now the industry standard/baseline for codesigning certs, even outside of driver signing. Sorry for the confusion!)

---

_Comment by @ffyring on 2025-11-19 09:27_

I just want to bump this issue / feature request. Currently I cannot fully use uv in my restrictive corporate environment (see #14401).

---

_Comment by @mattculler on 2025-11-24 13:48_

I too cannot fully integrate uv into my team's workflow until it distributes signed Windows executables.

---

_Comment by @EliteTK on 2026-01-08 21:54_

So, after a lot of confusion and a bit more investigation, it looks like we have two options:

* [Azure Artifact Signing](https://azure.microsoft.com/en-us/products/artifact-signing/) (formerly "Trusted Signing") which offers [non-EV](https://learn.microsoft.com/en-us/azure/artifact-signing/faq#does-artifact-signing-issue-ev-certificates) certificates but which are rooted at Microsoft. For this we can use the [trusted-signing-action](https://github.com/Azure/trusted-signing-action) or [SignTool.exe](https://learn.microsoft.com/en-us/dotnet/framework/tools/signtool-exe).
* [Azure Key Vault](https://azure.microsoft.com/en-us/products/key-vault/) (or some other compatible hosted HSM solution) with an EV certificate from one of the many providers (although it seems there's [some integration with DigiCert and GlobalSign](https://learn.microsoft.com/en-us/azure/key-vault/certificates/how-to-integrate-certificate-authority)). As far as I am aware there is no off the shelf github action, but there is [AzureSignTool](https://github.com/vcsjones/AzureSignTool/) and there's even a [guide](https://github.com/vcsjones/AzureSignTool/blob/main/WALKTHROUGH.md)[^1] for using it in Github CI.

So I guess the question is, are these Microsoft rooted non-EV certificates sufficient or would it be a waste of effort to not go down the EV path?

[^1]: Microsoft also have a guide: https://learn.microsoft.com/en-us/windows/msix/desktop/cicd-keyvault

---
