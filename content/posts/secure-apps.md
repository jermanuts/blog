---
title: 'Identifying Secure Software'
date: 2024-04-01T23:40:40+03:00
draft: false
tags: ['security']
---

A lot of people ask, "How do I know if an app is secure?" So, I decided to compile all the best practices developers should be following, so the end user could determine if their app is taking security seriously. Feel free to suggest any improvements [here](https://github.com/jermanuts/blog/discussions).

# Android

* Fewer permissions (shouldn't access resources it doesn't need).

* Provides hashes for releases for integrity checks; releases should also be signed by the developer's private key and then share its public public key to verify the signature for authenticity. [Resource](https://infra.apache.org/release-signing.html).

* Reproducible builds, only with [Guix](https://guix.gnu.org/) because it is [Full-Source bootstrappable](https://guix.gnu.org/en/blog/2024/identifying-software/).

* Lastly, audited. But it is not necessary, as it is expensive.

# Desktop

* It shouldn't be an Electron app; it is an [insecure](https://github.com/secureblue/secureblue/issues/193#issuecomment-1953323680) [pile](https://github.com/sickcodes/no-sandbox) of [shit](https://blog.doyensec.com/2022/09/27/electron-api-default-permissions.html) with a huge attack surface.

* Provides hashes for releases for integrity checks; releases should also be signed by the developer's private key and then share its public public key to verify the signature for authenticity. [Resource](https://infra.apache.org/release-signing.html).

* Reproducible builds, only with [Guix](https://guix.gnu.org/) because it is [Full-Source bootstrappable](https://guix.gnu.org/en/blog/2024/identifying-software/).

* Lastly, audited. But it is not necessary, as it is expensive.

# Web Apps

Never use [them](https://cronokirby.com/posts/2021/06/e2e_in_the_browser/). They are unreproducible by design and [insecure](https://www.devever.net/~hl/webcrypto), requires trusting the server always. Unless you are in control of that server on hardware you own, to mitigate any risks of VPS providers messing up with your machine, for example, when using disk encryption, the decryption key is stored in RAM while the disk is mounted. It is possible for the VPS provider, or anyone else that has access to the hypervisor running your VM, to dump the decryption keys from RAM, which would give the attacker full access to your unencrypted data (Cold boot attack).

## Extras, nice to have:

* Secure communication methods, instant messaging like Matrix, etc. and not some closed source proprietary software like [Discord](https://sneak.berlin/20200220/discord-is-not-an-acceptable-choice-for-free-software-projects/). Email with a high security rating on hardenize and internet.nl (to prevent spoofing, etc.), a high score for the website too, and of course, a public PGP key for email. 

* Full transparency and security disclosure, reporting procedure documented.

* Developers should be aware of [supply chain attacks](https://discuss.privacyguides.net/t/add-supply-chain-attacks-to-common-threats-page/17595/4) and patch stuff immediately, and compartmentalize the development environment.

* Warrant canary (in case of a service).

As for privacy, they shouldn't phone home and have a toggle to disable it and any telemetry when setting up the app initially.


----

Disclaimer: There will always be a chance of insecure code or vulnerabilities being found; what should really matter is responding to these issues quickly and patching them as soon as possible.



