---
title: 'Fully Encrypted Protocols (FEP) Overview'
date: 2023-10-02T18:02:12+03:00
draft: false
tags: ['proxies']
---

# Obfuscation methods

V2Ray, Trojan-GFW, ShadowSocks, Obfsproxy 

These methods strive to achieve something in common which is making it difficult for firewalls to differantiate the internet traffic, which are useful for countries that uses sophisticated deep packet inspection (DPI) such as China, Vietnam, Iran, Egypt, Russia and Uzbekistan. 

## V2Ray

[V2ray/V2fly](https://guide.v2fly.org/en_US/) is a multi-protocol proxy server and client (Framework). It can encapsulate any protocol into any other, as well as build protocol/proxy chains. It has two of its own protocols: vmess and vless, and all the rest are third-party ones for connecting to existing servers using other protocols.

Vmess is a full-fledged tunneling protocol, with its own encryption “built” into the protocol. It only works over TCP, all UDP traffic is also encapsulated in TCP (unlike Shadowsocks, where TCP traffic is carried over TCP and UDP traffic is carried over UDP). Protocol encryption is time-bound: the time on the server and client must be synchronized, otherwise the connection will not be established. Vmess does not use TLS and does not appear to be TLS traffic (but can be encapsulated in TLS if necessary).

Vless is a continuation, a development of vmess, in which the protocol encryption layer was removed, which made it possible to get rid of the time reference. The protocol was created to be used with TLS encapsulation, so that it was possible to determine the type of TLS request (vless or a regular TLS request from a browser or other program) and redirect non-vless traffic to another program or to another port, for example to disguise the proxy as a live website: both the proxy and the website will work on the same port.

### XTLS? 

[Xray-core](xtls) is a superset of v2ray-core, with better overall performance and enhancements such as XTLS, and it'scompletelycompatible with v2ray-core functionality and configuration.

- Only one executable file, including ctl functionality, run is the default command

    - Configuration is ~~completely~~ compatible, environment variables and API calls need to be changed to start with XRAY_

    - Exposed raw protocol's ReadV on all platforms

    - Provides complete VLESS & Trojan XTLS support, both with ReadV

    - Provides multiple XTLS flow control modes, unrivaled performance!

## Trojan-GFW 

[Trojan](https://trojan-gfw.github.io/trojan/) works on a similar TLS encapsulation principle to vless, the only difference is in the protocol. It appeared early enough to demonstrate the principle of “invisible” encapsulation in TLS. Here's what the authors write:

>Trojan is not a fixed program or protocol. It’s an idea, an idea that imitating the most common service, to an extent that it behaves identically, could help you get across the Great FireWall permanently, without being identified ever. We are the GreatER Fire; we ship Trojan Horses.

## ShadowSocks

[Shadowsocks](https://shadowsocks.org/) is a secure split proxy loosely based on SOCKS5.

`client <---> ss-local <--[encrypted]--> ss-remote <---> target`

The Shadowsocks local component (ss-local) acts like a traditional SOCKS5 server and provides proxy service to clients. It encrypts and forwards data streams and packets from the client to the Shadowsocks remote component (ss-remote), which decrypts and forwards to the target. Replies from target are similarly encrypted and relayed by ss-remote back to ss-local, which decrypts and eventually returns to the original client.

### Difference between V2Ray and ShadowSocks

The difference is still that Shadowsocks is just a simple proxy tool; it is a protocol of encryption. However, V2Ray is designed as a platform, and any developer can use the modules provided by V2Ray to develop new proxy software.

Shadowsocks is a single proxy protocol, and V2Ray is more complicated than a single protocol proxy. Sounds a bit bleak to Shadowsocks? of course not! From another point of view, Shadowsocks is easy to deploy, and V2Ray has more complicated configurations while deploying.

## Obfsproxy

[Obfsproxy](https://2019.www.torproject.org/docs/pluggable-transports) is a project made by Tor which can be used to obfuscate your internet traffic. Initially, it was only built for Tor, Obfsproxy has a feature that allows developers to design, deploy, and test more obfuscating layers without using Tor. So, Obfsproxy has started being used to [obfuscate OpenVPN traffic](https://community.openvpn.net/openvpn/wiki/TrafficObfuscation) too.

It is a protocol obfuscation layer for TCP protocols.  Its purpose is tokeep a third party from telling what protocol is in use based on message contents.

Unlike obfs3, obfs4 attempts to provide authentication and data integrity,though it is still designed primarily around providing a layer ofobfuscation for an existing authenticated protocol like SSH or TLS.

Like obfs3 and ScrambleSuit, the protocol has 2 phases: in the first phaseboth parties establish keys. In the second, the parties exchangesuper-enciphered traffic. 

You can read the whole spec [here](https://github.com/Yawning/obfs4/blob/master/doc/obfs4-spec.txt).

## Conclusion

The downsides of these obfuscations protocols is that their [security](https://bamsoftware.com/papers/fep-flaws/) implications [generally](https://github.com/net4people/bbs/issues/36#issuecomment-645422316) [less](https://www.petsymposium.org/foci/2023/foci-2023-0002.php) [explored](https://www.petsymposium.org/foci/2023/foci-2023-0004.php) than VPNs. Wireguard being known for its small codebase, faster and using [state of art cryptography](https://www.wireguard.com/formal-verification/), and OpenVPN for its popularity and decent security, are the ones that are most frequently [audited](https://www.usenix.org/conference/usenixsecurity22/presentation/xue-diwen), but they have distinct characteristics that can be blocked without needing to know about every server.

In my findings, [NaïveProxy](https://github.com/klzgrad/naiveproxy) seems to be the most secure obfuscation proxy.

If you want to stay up-to-dated with the latest censorshop methods conducted in your country and understand it better, I recommend you to choose your country label at https://github.com/net4people/bbs/labels and the check posts shared by developers and researchers.

#### Resources:

https://gfw.report/

https://censorbib.nymity.ch

https://www.petsymposium.org/foci/2023/

https://danglingpointer.fun/2022/09/09/GFWHistory/

https://github.com/net4people/bbs/issues/129

https://github.com/net4people/bbs/issues/36