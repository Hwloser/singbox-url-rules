# singbox-url-rules

Public-safe remote rule profiles for personal VPN clients.

This repository intentionally does not contain VLESS UUIDs, Reality private keys,
subscription URLs, or any other proxy credentials.

## Shadowrocket

Profile file:

```text
shadowrocket/shadowrocket-cn-direct.conf
```

Raw URL after pushing to GitHub:

```text
https://raw.githubusercontent.com/Hwloser/singbox-url-rules/master/shadowrocket/shadowrocket-cn-direct.conf
```

Purpose:

- Keep private/local addresses direct.
- Keep the VPN server IP direct to avoid proxy loops.
- Keep common China DNS and DoH endpoints direct, so requests such as
  `223.5.5.5:443` do not go through this VPN node.
- Keep China domains and China IPs direct.
- Send everything else to the selected proxy.
- Block common WebRTC/STUN/TURN domains to reduce direct peer-discovery leaks.

Use this as a Shadowrocket remote config/rule profile while keeping the actual
VLESS Reality node stored locally in Shadowrocket.

