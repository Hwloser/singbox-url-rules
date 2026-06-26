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

Direct local, Hong Kong, China, Apple App Store, China DNS, China AI, and China
daily app traffic:

```text
https://raw.githubusercontent.com/Hwloser/singbox-url-rules/main/clash-verge/ruleset-direct.yaml
```

Proxy foreign AI, Gemini, Google AI dependencies, foreign social/messaging,
GitHub/developer platforms, TradingView, Wise, non-HK overseas brokerages, OKX,
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

If Clash Verge keeps using stale cached providers, replace the provider URLs
with cache-busting URLs and new local paths:

```text
https://raw.githubusercontent.com/Hwloser/singbox-url-rules/master/clash-verge/ruleset-reject.yaml?cb=2026062601-ifastgb
https://raw.githubusercontent.com/Hwloser/singbox-url-rules/master/clash-verge/ruleset-direct.yaml?cb=2026062601-ifastgb
https://raw.githubusercontent.com/Hwloser/singbox-url-rules/master/clash-verge/ruleset-proxy.yaml?cb=2026062601-ifastgb
```

## Rule Order

Use this order in a Clash/Mihomo profile:

```yaml
rules:
  - RULE-SET,privacy-reject,REJECT
  - DST-PORT,3478,REJECT
  - DST-PORT,5349,REJECT
  - DST-PORT,19302-19309,REJECT
  - RULE-SET,foreign-proxy,PROXY
  - DOMAIN-SUFFIX,hk,DIRECT
  - GEOIP,HK,DIRECT,no-resolve
  - RULE-SET,cn-direct,DIRECT
  - GEOSITE,cn,DIRECT
  - GEOIP,CN,DIRECT,no-resolve
  - MATCH,PROXY
```

The explicit proxy provider must stay before the broad CN direct provider. This
keeps domains such as Google-owned `.cn` hosts or `okx.com.cn` on the proxy path
when they are intentionally listed in `foreign-proxy`.

The explicit proxy provider is placed before the broad HK/CN direct rules. This
keeps intentionally proxied services such as BIT, Bitget, Wise, OKX, Binance,
and Google on the proxy path even if they use mixed regional infrastructure.

The `foreign-proxy` provider includes explicit Gemini client support for
Gemini web/app/API traffic and Google login/API dependencies, including
`gemini.google.com`, `aistudio.google.com`, `ai.google.dev`,
`generativelanguage.googleapis.com`, `accounts.google.com`,
`oauth2.googleapis.com`, `googleapis.com`, `gstatic.com`,
`geministatic.com`, and `googleusercontent.com`.

It also explicitly covers common foreign SaaS, design, developer, storage,
payment, and content platforms such as Notion, Figma, Canva, Dropbox,
Atlassian, Airtable, Vercel, Netlify, Docker Hub, npm, PyPI, Stripe, PayPal,
Medium, Substack, Wikipedia, Spotify, and Netflix.
