# Clash Verge / Mihomo

Clash Verge cannot directly import the Shadowrocket `.conf` profile in this
repository. Clash Verge uses Clash/Mihomo YAML, so use the rule-provider files
below from a Clash/Mihomo profile.

These files are public-safe and contain routing rules only. They do not contain
VLESS UUIDs, Reality keys, subscription URLs, or node credentials.

## Rule Provider URLs

Reject WebRTC/STUN/TURN privacy endpoints:

```text
https://raw.githubusercontent.com/Hwloser/singbox-url-rules/main/clash-verge/ruleset-reject.yaml
```

Direct local, China, Apple App Store, China DNS, China AI, and China daily app
traffic:

```text
https://raw.githubusercontent.com/Hwloser/singbox-url-rules/main/clash-verge/ruleset-direct.yaml
```

Proxy foreign AI, Gemini, Google AI dependencies, foreign social/messaging,
GitHub/developer platforms, TradingView, Wise, overseas brokerages, OKX,
Binance, and BiyaPay:

```text
https://raw.githubusercontent.com/Hwloser/singbox-url-rules/main/clash-verge/ruleset-proxy.yaml
```

Template profile:

```text
https://raw.githubusercontent.com/Hwloser/singbox-url-rules/main/clash-verge/profile-template.yaml
```

The template contains placeholder node fields. Do not publish a filled profile
because it would expose your VLESS Reality credentials.

## Rule Order

Use this order in a Clash/Mihomo profile:

```yaml
rules:
  - RULE-SET,privacy-reject,REJECT
  - DST-PORT,3478,REJECT
  - DST-PORT,5349,REJECT
  - DST-PORT,19302-19309,REJECT
  - RULE-SET,cn-direct,DIRECT
  - RULE-SET,foreign-proxy,PROXY
  - GEOSITE,cn,DIRECT
  - GEOIP,CN,DIRECT,no-resolve
  - MATCH,PROXY
```

The `foreign-proxy` provider includes explicit Gemini client support for
Gemini web/app/API traffic and Google login/API dependencies, including
`gemini.google.com`, `aistudio.google.com`, `ai.google.dev`,
`generativelanguage.googleapis.com`, `accounts.google.com`,
`oauth2.googleapis.com`, `googleapis.com`, `gstatic.com`,
`geministatic.com`, and `googleusercontent.com`.
