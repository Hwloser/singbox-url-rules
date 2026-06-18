# singbox-url-rules

Public-safe rule profiles for personal VPN clients.

This repository only stores client-side routing rules. It intentionally does not
store VLESS URLs, UUIDs, Reality keys, subscription URLs, ntfy URLs, or any other
proxy credentials.

## Shadowrocket

Profile:

```text
shadowrocket/shadowrocket-cn-direct.conf
```

Recommended import URL:

```text
https://raw.githubusercontent.com/Hwloser/singbox-url-rules/main/shadowrocket/shadowrocket-cn-direct.conf
```

Cache-busting URL for immediate refresh after edits:

```text
https://raw.githubusercontent.com/Hwloser/singbox-url-rules/main/shadowrocket/shadowrocket-cn-direct.conf?cb=2026061802-apple
```

Fallback URL:

```text
https://raw.githubusercontent.com/Hwloser/singbox-url-rules/master/shadowrocket/shadowrocket-cn-direct.conf
```

jsDelivr mirror:

```text
https://cdn.jsdelivr.net/gh/Hwloser/singbox-url-rules@main/shadowrocket/shadowrocket-cn-direct.conf
```

Note: GitHub raw can cache 404 or stale content for a few minutes. jsDelivr can
cache branch content longer. If an update does not appear immediately, use the
cache-busting GitHub raw URL first.

## Intended Behavior

Use this profile with your VLESS Reality node stored locally in Shadowrocket.

Rule behavior:

- Private and local networks go `DIRECT`.
- The VPN server IP goes `DIRECT` to avoid proxy loops.
- China DNS and DoH endpoints go `DIRECT`, so CN DNS requests such as
  `223.5.5.5:443` do not hit the VPN node.
- Apple App Store, Apple CDN, Apple ID, and iCloud basics go `DIRECT`, so app
  downloads and updates keep working when the profile is enabled.
- Mainland China AI tools go `DIRECT`, including DeepSeek, Kimi, Tongyi/Qwen,
  Doubao, Baidu Wenxin, Tencent Yuanbao/Hunyuan, Zhipu/ChatGLM, MiniMax,
  iFlyTek Spark, Metaso, and similar domestic services.
- Mainland China apps, payments, video, and commerce go `DIRECT`, including
  Alipay, WeChat/Weixin, Douyin, Bilibili, Alibaba/Taobao/Tmall, JD,
  Pinduoduo, Meituan, and common domestic video platforms.
- Common non-China AI tools go `PROXY` before CN/GEOIP rules are evaluated.
- Foreign social media and developer platforms go `PROXY`, including X/Twitter,
  Instagram, Facebook, Threads, WhatsApp, Telegram, Reddit, Discord, LinkedIn,
  GitHub, GitLab, Bitbucket, and Stack Overflow.
- TradingView goes `PROXY`, including charting, screeners, explore-style pages,
  widgets, websocket/pushstream, and static assets.
- Wise, Hong Kong/overseas brokerages, OKX, Binance, and BiyaPay go `PROXY` for
  route consistency through the same personal VPN exit.
- China domains and China IPs go `DIRECT`.
- Everything else goes `PROXY`.
- Common WebRTC/STUN/TURN endpoints are rejected as a generic privacy guard.

## AI Traffic

Mainland China AI services are intentionally direct. This keeps domestic AI
traffic off the VPN node and avoids routing Chinese AI traffic through the
overseas exit.

Mainland China daily-use apps are also intentionally direct, especially payment,
messaging, video, shopping, delivery, and map services. These are domestic
traffic and should not consume the VPN node.

The profile explicitly forces common foreign AI tools through the selected proxy,
including OpenAI/ChatGPT, Claude, Gemini, Perplexity, Copilot, Cursor,
Hugging Face, OpenRouter, Mistral, Midjourney, Runway, and related service
domains.

Unknown non-China domains are still covered by `FINAL,PROXY`. This cannot prove
coverage of every future AI vendor domain, but the default route is proxy unless
a rule matches `DIRECT` first.

## Foreign Social and Developer Platforms

Foreign social media and developer platforms are intentionally forced through
the proxy to avoid mixed direct/proxy resource loading and to keep the overseas
exit consistent. This includes X/Twitter and its media/CDN/helper domains,
Instagram, Facebook, Threads, WhatsApp, Telegram, Reddit, Discord, LinkedIn,
GitHub, GitLab, Bitbucket, and Stack Overflow.

## Apple App Store

Apple domains are intentionally direct. This avoids App Store download failures
seen when all Apple CDN traffic goes through the VPN node.

This means some Apple traffic will not use the VPN while this profile is active.
That is expected for this rule set.

## TradingView

TradingView is intentionally forced through the proxy before CN/GEOIP rules.
This helps avoid partial page failures where the main page loads but resources
for charting, scanners, widgets, or explore-style pages are split across direct
and proxy paths.

## Financial Services, Brokerages, and Crypto Exchanges

Wise, Hong Kong/overseas brokerages, OKX, Binance, and BiyaPay are intentionally
forced through the proxy so account sessions, API calls, static resources, and
websocket traffic use the same personal VPN exit. Brokerages include Longbridge,
Futu/Futubull, Moomoo, Tiger Brokers, Webull, IBKR, Saxo, Firstrade, uSMART, and
SoFi HK.

Use this only for a stable fixed exit that you are allowed to use. Do not use it
to bypass service eligibility, regional restrictions, sanctions, KYC rules, or
platform risk controls.

## Import Steps

1. Keep the actual VLESS Reality node configured locally in Shadowrocket.
2. Add this file as a remote config/profile URL.
3. Enable rule mode.
4. Refresh the remote config after changes.
5. If App Store still fails, disable/re-enable Shadowrocket once so iOS clears
   the old connection state.

## Security Notes

- This repo is safe to keep public as long as it only contains rules.
- Do not commit VLESS URLs, UUIDs, Reality private keys, ntfy URLs, or generated
  client config files.
- The profile exposes the VPN server IP because it is needed for direct loop
  prevention. That IP is not an authentication secret, but it is metadata.
