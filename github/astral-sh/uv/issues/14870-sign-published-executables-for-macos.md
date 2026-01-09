---
number: 14870
title: Sign published executables for macOS
type: issue
state: open
author: Jaharmi
labels:
  - enhancement
assignees: []
created_at: 2025-07-24T12:42:12Z
updated_at: 2025-11-10T19:01:30Z
url: https://github.com/astral-sh/uv/issues/14870
synced_at: 2026-01-07T13:12:19-06:00
---

# Sign published executables for macOS

---

_Issue opened by @Jaharmi on 2025-07-24 12:42_

### Summary

The `uv` and `uvx` tools do not carry a code signature from Astral on macOS, as of version 0.8.2 checked on 2025-07-24 on macOS 15.5.

```
❯ uv --version
uv 0.8.2 (21fadbcc1 2025-07-22)

❯ sw_vers
ProductName:		macOS
ProductVersion:		15.5
BuildVersion:		24F74
```

I could not find a similar open or closed request regarding adding a macOS code signature to `uv` and `uvx` in the issue tracker. Issue #10366 is a similar request for Windows. 

Code signing from the developer is preferred and contributes to trust in these tools when running on macOS / Darwin. Code signing could potentially be done with other PKI, but for this request, the best result would be an Apple Developer ID signature.
 
1. Signing versions of `uv` and `uvx` on macOS would provide proof that the release came from Astral (or Astral’s build/release pipeline). CI/CD pipeline tools offer support for code signing actions.
2. The signature would enable verification that the tools have not been modified since their release.
3. The signature can be used in Apple’s anti-malware / threat protection processes.
4. The valid signature and its Team ID can be used by automation for IT teams, like [AutoPkg](https://github.com/autopkg/autopkg), to verify the downloaded releases when discovering new versions, packaging them, and preparing them for organizational deployments.

Further, some organizations may either require apps/tools on macOS to carry a code signature from the developer, or encourage use of software with signatures over those without.

### Example


Current shell script download on Apple Silicon:

```
❯ codesign -dvvv ~/.local/bin/uv ~/.local/bin/uvx
Executable=/Users/jeremy/.local/bin/uv
Identifier=uv-ba525b85e50d75a6
Format=Mach-O thin (arm64)
CodeDirectory v=20400 size=290776 flags=0x20002(adhoc,linker-signed) hashes=9083+0 location=embedded
Hash type=sha256 size=32
CandidateCDHash sha256=43d7050030a9c5fa69c2f834f9b8374352da16b0
CandidateCDHashFull sha256=43d7050030a9c5fa69c2f834f9b8374352da16b03fd0c2ad39c6d41efabf5505
Hash choices=sha256
CMSDigest=43d7050030a9c5fa69c2f834f9b8374352da16b03fd0c2ad39c6d41efabf5505
CMSDigestType=2
CDHash=43d7050030a9c5fa69c2f834f9b8374352da16b0
Signature=adhoc
Info.plist=not bound
TeamIdentifier=not set
Sealed Resources=none
Internal requirements=none
Executable=/Users/jeremy/.local/bin/uvx
Identifier=uvx-67251bcd6ebf433f
Format=Mach-O thin (arm64)
CodeDirectory v=20400 size=2744 flags=0x20002(adhoc,linker-signed) hashes=82+0 location=embedded
Hash type=sha256 size=32
CandidateCDHash sha256=52f9a4d977d105b9ab5e232aae9f26e85edfce85
CandidateCDHashFull sha256=52f9a4d977d105b9ab5e232aae9f26e85edfce855425d1a71cd9740129ae2829
Hash choices=sha256
CMSDigest=52f9a4d977d105b9ab5e232aae9f26e85edfce855425d1a71cd9740129ae2829
CMSDigestType=2
CDHash=52f9a4d977d105b9ab5e232aae9f26e85edfce85
Signature=adhoc
Info.plist=not bound
TeamIdentifier=not set
Sealed Resources=none
Internal requirements=none
```

Current binary download for Apple Silicon:

```
❯ codesign -dvvv ~/Downloads/uv-aarch64-apple-darwin/uv ~/Downloads/uv-aarch64-apple-darwin/uvx
Executable=/Users/jeremy/Downloads/uv-aarch64-apple-darwin/uv
Identifier=uv-ba525b85e50d75a6
Format=Mach-O thin (arm64)
CodeDirectory v=20400 size=290776 flags=0x20002(adhoc,linker-signed) hashes=9083+0 location=embedded
Hash type=sha256 size=32
CandidateCDHash sha256=43d7050030a9c5fa69c2f834f9b8374352da16b0
CandidateCDHashFull sha256=43d7050030a9c5fa69c2f834f9b8374352da16b03fd0c2ad39c6d41efabf5505
Hash choices=sha256
CMSDigest=43d7050030a9c5fa69c2f834f9b8374352da16b03fd0c2ad39c6d41efabf5505
CMSDigestType=2
CDHash=43d7050030a9c5fa69c2f834f9b8374352da16b0
Signature=adhoc
Info.plist=not bound
TeamIdentifier=not set
Sealed Resources=none
Internal requirements=none
Executable=/Users/jeremy/Downloads/uv-aarch64-apple-darwin/uvx
Identifier=uvx-67251bcd6ebf433f
Format=Mach-O thin (arm64)
CodeDirectory v=20400 size=2744 flags=0x20002(adhoc,linker-signed) hashes=82+0 location=embedded
Hash type=sha256 size=32
CandidateCDHash sha256=52f9a4d977d105b9ab5e232aae9f26e85edfce85
CandidateCDHashFull sha256=52f9a4d977d105b9ab5e232aae9f26e85edfce855425d1a71cd9740129ae2829
Hash choices=sha256
CMSDigest=52f9a4d977d105b9ab5e232aae9f26e85edfce855425d1a71cd9740129ae2829
CMSDigestType=2
CDHash=52f9a4d977d105b9ab5e232aae9f26e85edfce85
Signature=adhoc
Info.plist=not bound
TeamIdentifier=not set
Sealed Resources=none
Internal requirements=none
```

---

_Label `enhancement` added by @Jaharmi on 2025-07-24 12:42_

---

_Comment by @FishAlchemist on 2025-07-24 17:51_

Regarding signing executable files, Windows was also mentioned previously.
* https://github.com/astral-sh/uv/issues/10336

But does signing actually provide enough benefits to outweigh the cost? From what I understand, digital signatures for programs aren't cheap.

---

_Comment by @zanieb on 2025-07-24 19:25_

The cost of the developer license would be fairly negligible. The work to implement automation for this is the bigger lift here. I'm not sure when we'll be able to pursue it.

---

_Comment by @kariharju on 2025-10-01 10:55_

@zanieb would you be open for contributions?

Asking because we've gone through the pain of signing executables and installers.
We also are hurting on uv executables not being signed.

---

_Comment by @zanieb on 2025-10-01 15:09_

If you want to make a contribution, that could be helpful. We'd need to audit anything security related like this pretty closely, but if you want to do the proof-of-concept to sign our distributions we're happy to take a look. I think the technical challenges are mostly around signing the binaries packaged in our wheels.

---

_Comment by @kariharju on 2025-10-01 15:35_

Macos signing was complex enough that I had to make a .sh rather than coding it all to GHA 😅

sign_and_notarize.sh
```
#!/bin/bash
set -e  # Exit immediately if a command exits with a non-zero status

# Usage: ./script/sign_and_notarize.sh <path_to_executable> <output_directory> <entitlements_file>

TARGET_EXECUTABLE="$1"
OUTPUT_DIRECTORY="$2"
ENTITLEMENTS_FILE="$3"

if [ -z "$TARGET_EXECUTABLE" ]; then
  echo "Error: No target executable provided."
  echo "Usage: $0 <path_to_executable> <output_directory> <entitlements_file>"
  exit 1
fi

if [ -z "$OUTPUT_DIRECTORY" ]; then
  echo "Error: No output directory provided."
  echo "Usage: $0 <path_to_executable> <output_directory> <entitlements_file>"
  exit 1
fi

# Check if the output directory exists, create if it doesn't
if [ ! -d "$OUTPUT_DIRECTORY" ]; then
  mkdir -p "$OUTPUT_DIRECTORY"
  echo "Created output directory: $OUTPUT_DIRECTORY"
fi

# Sign MacOS binary
echo "Signing MacOS binary..."
security create-keychain -p "$MACOS_SIGNING_CERT_PASSWORD" build.keychain
security default-keychain -s build.keychain
security unlock-keychain -p "$MACOS_SIGNING_CERT_PASSWORD" build.keychain
echo "$MACOS_SIGNING_CERT" | base64 --decode -o cert.p12
security import cert.p12 -A -P "$MACOS_SIGNING_CERT_PASSWORD"
security set-key-partition-list -S apple-tool:,apple: -s -k "$MACOS_SIGNING_CERT_PASSWORD" build.keychain

codesign --verbose=4 --entitlements "$ENTITLEMENTS_FILE" --deep --force -o runtime -s "$MACOS_SIGNING_CERT_NAME" --timestamp "$TARGET_EXECUTABLE"
# Verify the code signature and the contained executables
codesign --verify --verbose=2 --deep "$TARGET_EXECUTABLE"
# Display the signature information
codesign --verify --verbose=2 --display "$TARGET_EXECUTABLE"

# Notarize (zipped because notarization does not allow executable files)
zip temp_signing.zip "$TARGET_EXECUTABLE"
xcrun notarytool submit --apple-id "$APPLEID" --team-id "$APPLETEAMID" --password "$APPLEIDPASS" temp_signing.zip
unzip temp_signing.zip -d "$OUTPUT_DIRECTORY"

echo "Signing and notarization process completed."
```
..disclaimer that this maybe outdated to some extent, we've been running this successfully of years.
Well actually every 2 years Apple seems to like to change the signing around, just to keep us awake I think.

The related GHA using that would be:
```
- name: Sign and Notarize
        run: |
          chmod +x ./scripts/sign_and_notarize.sh
          ./scripts/sign_and_notarize.sh <your executable here> ./dist/ ./entitlements.mac.plist
        env:
          APPLEID: ${{ secrets.MACOS_APP_ID_FOR_NOTARIZATION }}
          APPLEIDPASS: ${{ secrets.MACOS_APP_ID_PASSWORD_FOR_NOTARIZATION }}
          APPLETEAMID: ${{ secrets.MACOS_TEAM_ID_FOR_NOTARIZATION }}
          MACOS_SIGNING_CERT: ${{ secrets.MACOS_SIGNING_CERT }}
          MACOS_SIGNING_CERT_PASSWORD: ${{ secrets.MACOS_SIGNING_CERT_PASSWORD }}
          MACOS_SIGNING_CERT_NAME: ${{ secrets.MACOS_SIGNING_CERT_NAME }}
```

..and to get the values for the above you have to use two different Apple portals, after you get the developer IDs.

If you get there one heads-up on GHA is that the macos fleet is still very weird. Sometimes the sing &notarize just decides to take 5-10 minutes and sometimes it passes in under a minute. Cannot tell the amount of times this happens on a critical release days, when minutes feel like hours 😅

---

_Comment by @idavidrein on 2025-10-31 20:27_

I would find this extremely useful FYI!

---

_Comment by @PaarthShah on 2025-11-10 19:01_

> I would find this extremely useful FYI!

Yep, organizations that use strict security postures for software that's installed on user macbooks would very much like to have this done.

If there's any testing to this end that I can contribute, please feel free to reach out


---
